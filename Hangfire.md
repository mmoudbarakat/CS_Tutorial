# Hangfire in .NET

---

## 1. What is Hangfire?

- **Hangfire** is a **framework** for background job processing in **.NET**.
    
- It allows you to run background tasks in an **asynchronous** manner, such as **delayed**, **recurring**, or **fire-and-forget** jobs.
    
- Hangfire works by using a **persistent storage** (e.g., SQL Server, Redis, etc.) to keep track of jobs, so if the application crashes, the jobs are not lost and will be retried once the system is back.
    

---

## 2. Key Features of Hangfire

- **Background Jobs**: Executes jobs outside the request-response cycle.
    
- **Reliable**: Jobs are stored in a persistent data store and will be retried in case of failure.
    
- **Scalable**: Supports multiple workers to handle jobs in parallel.
    
- **Recurring Jobs**: Schedule jobs to run at fixed intervals.
    
- **Dashboard**: Provides a **web-based dashboard** to monitor job execution, failures, and retries.
    
- **Supports Various Job Types**:
    
    - **Fire-and-forget jobs**: Execute once and forget.
        
    - **Delayed jobs**: Execute after a specified delay.
        
    - **Recurring jobs**: Execute on a recurring schedule.
        

---

## 3. Hangfire Job Types

### 3.1 Fire-and-Forget Jobs

- These jobs are executed **immediately** after they are added and don’t require any input from the caller once they are triggered.
    

**Example**:

```csharp
BackgroundJob.Enqueue(() => Console.WriteLine("Fire-and-forget job"));
```

- **Use case**: Sending an email, logging, etc., that doesn't need to block the main thread.
    

### 3.2 Delayed Jobs

- These jobs are executed after a **specific delay**.
    

**Example**:

```csharp
BackgroundJob.Schedule(() => Console.WriteLine("Delayed job"), TimeSpan.FromMinutes(10));
```

- **Use case**: Send a reminder email after a specified delay.
    

### 3.3 Recurring Jobs

- These jobs are executed at specified **intervals** (e.g., every minute, daily, etc.).
    

**Example**:

```csharp
RecurringJob.AddOrUpdate(() => Console.WriteLine("Recurring job"), Cron.Hourly);
```

- **Use case**: Generate reports at the end of each day, or clean up old data periodically.
    

---

## 4. How to Set Up Hangfire in a .NET Application

### 4.1 Install Hangfire NuGet Package

Run the following command in the **NuGet Package Manager**:

```bash
Install-Package Hangfire
```

Or use the **.NET CLI**:

```bash
dotnet add package Hangfire
```

### 4.2 Configure Hangfire in Startup

In **Startup.cs** (or **Program.cs** in .NET 6+):

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        // Add Hangfire services to DI container
        services.AddHangfire(config => config.UseSqlServerStorage("YourConnectionString"));
        services.AddHangfireServer(); // Add Hangfire server to process jobs
    }

    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        // Set up the Hangfire dashboard
        app.UseHangfireDashboard();

        // Run Hangfire server
        app.UseHangfireServer();
    }
}
```

- **UseSqlServerStorage**: Configures Hangfire to use **SQL Server** for storing job data.
    
- **UseHangfireDashboard**: Provides a web interface to view job progress and status.
    
- **UseHangfireServer**: Starts a background worker to process jobs.
    

---

## 5. Hangfire Dashboard

- The **Hangfire Dashboard** is a **web-based interface** that lets you monitor background jobs.
    
- By default, it's available at the **/hangfire** URL of your application.
    

### 5.1 Features of the Dashboard

- **View job details**: Status of jobs, including whether they are running, completed, or failed.
    
- **Retry failed jobs**: Jobs that fail can be retried manually.
    
- **Job history**: View past job executions and their results.
    
- **Real-time monitoring**: Track job progress and successful/failed executions.
    

---

## 6. Handling Job Failures and Retries

Hangfire provides automatic retry mechanisms for jobs that fail.

### 6.1 Automatic Retries

Hangfire will automatically retry failed jobs based on the **job retry configuration**. By default, jobs will be retried up to **10 times**.

You can configure the **retry count**:

```csharp
public class MyJob
{
    public void ProcessData()
    {
        // Job logic
    }
}

BackgroundJob.Enqueue<MyJob>(job => job.ProcessData());

// Configuring retry behavior
GlobalJobFilters.Filters.Add(new AutomaticRetryAttribute { Attempts = 5 });
```

- **AutomaticRetryAttribute**: Specifies how many times a job should be retried on failure.
    

### 6.2 Custom Job Retry Logic

You can also define **custom retry logic** to handle errors or define a retry strategy.

```csharp
public class MyJob
{
    public void ProcessData()
    {
        try
        {
            // Logic that might throw exceptions
        }
        catch (Exception ex)
        {
            // Custom error handling and retry logic
            throw; // Re-throws the exception for Hangfire to retry
        }
    }
}
```

---

## 7. Job Expiration

Jobs can have an **expiration time** after which they will be automatically deleted from the storage.

```csharp
BackgroundJob.Enqueue(() => Console.WriteLine("This job will expire soon"));
```

You can configure the expiration time in Hangfire's settings:

```csharp
services.AddHangfire(config => config.UseSqlServerStorage("YourConnectionString").WithJobExpirationTime(TimeSpan.FromDays(7)));
```

---

## 8. Dependency Injection with Hangfire

Hangfire works well with **dependency injection (DI)** in **ASP.NET Core**. You can inject services into your background jobs.

```csharp
public class MyJob
{
    private readonly IEmailService _emailService;

    public MyJob(IEmailService emailService)
    {
        _emailService = emailService;
    }

    public void SendEmail()
    {
        _emailService.Send("email@example.com", "Hello World");
    }
}

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddTransient<IEmailService, EmailService>();
    }
}

// Enqueueing the job
BackgroundJob.Enqueue<MyJob>(job => job.SendEmail());
```

In this case, **MyJob** has a dependency on **IEmailService**, which will be injected by the **ASP.NET Core DI container**.

---

## 9. Advanced Features of Hangfire

### 9.1 Job Progress

Hangfire allows you to track the progress of a background job and report it back to clients.

```csharp
public void ProcessData(IJobCancellationToken cancellationToken)
{
    for (int i = 0; i < 100; i++)
    {
        cancellationToken.ThrowIfCancellationRequested();
        // Report progress
        BackgroundJob.ContinueWith(jobId, () => Console.WriteLine($"Progress: {i}%"));
    }
}
```

### 9.2 Continuations

You can define a job that runs **after another job completes** successfully.

```csharp
var jobId = BackgroundJob.Enqueue(() => Console.WriteLine("Job 1"));

BackgroundJob.ContinueWith(jobId, () => Console.WriteLine("Job 2"));
```

In this case, **Job 2** will run after **Job 1** finishes successfully.

---

## 10. Best Practices for Using Hangfire

- **Use persistent storage** for Hangfire to ensure job data is not lost on application restart.
    
- **Monitor job execution** using the Hangfire Dashboard to ensure jobs are completing successfully.
    
- **Limit retries** to prevent job failures from being retried indefinitely.
    
- **Schedule periodic jobs** using Hangfire's recurring job feature for regular maintenance tasks (e.g., database cleanup).
    
- **Handle exceptions gracefully** in background jobs to prevent job failures from affecting the entire application.
    

---

## 11. Summary

- **Hangfire** allows you to run background jobs, execute them **asynchronously**, and manage them with **retry mechanisms**, **recurring tasks**, and **job monitoring** through the dashboard.
    
- It is a **powerful tool** for handling background operations such as sending emails, processing large data, and performing periodic tasks in **.NET applications**.
    
- The integration with **ASP.NET Core** and **dependency injection** makes it easy to work with background jobs in **enterprise applications**.
    

---
