**Comments:**
Following Code hits DB twice.
Following Code loads entire object in memory.
Following Code does not explicitly use AsTracking() not allowing object to update state from DB.

> [!example] Code
>            var crs = await context.Courses
>            .FirstOrDefaultAsync(c => c.Id == id);
>            course.Name = crs.Name;
>            course.Description = crs.Description;
>            course.Hour = crs.Hour;
>            await context.SaveChangesAsync();


