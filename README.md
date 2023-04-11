# Identity_Core

### Package Install 
``` c# 
    Microsoft.Extensions.Identity.Stores
    Microsoft.EntityFrameworkCore
    Microsoft.EntityFrameworkCore.Tools
    Microsoft.AspNetCore.Identity.EntityFrameworkCore
    Microsoft.EntityFrameworkCore.SqlServer
```

### Create User Model Have Proerty User (AppUser.cs)
``` c#
    public class AppUser:IdentityUser
    {
        public string? FirstName { get; set; }
        public string? LastName { get; set; }
    }

```

### Creaete ApplicationDbContext To Connect With Database (ApplicationDbContext.cs)
``` c#
    public class ApplicationDbContext:IdentityDbContext<AppUser ,IdentityRole,string>
    {
        public ApplicationDbContext(DbContextOptions options):base(options)
        {
            
        }
    }
```

### Create Configration With Database (Startup.cs)
``` c#
    // Add Database Configration
    services.AddDbContext<ApplicationDbContext>(options =>
     {
          options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection"));
      });
      
    //Add Identity Configration 
            services.AddIdentity<AppUser,IdentityRole>()
                //To Define Where Want To Save Data
                .AddEntityFrameworkStores<ApplicationDbContext>();
            // Configure IdentityOptions
            
            private readonly string corsPolicy = "AllowAnyOrigin";
            
            services.Configure<IdentityOptions>(options =>
            {
                // Password settings
                options.Password.RequireDigit = true;
                options.Password.RequiredLength = 8;
                options.Password.RequireNonAlphanumeric = true;
                options.Password.RequireUppercase = true;
                options.Password.RequireLowercase = true;

                // Lockout settings
                options.Lockout.DefaultLockoutTimeSpan = TimeSpan.FromMinutes(5);
                options.Lockout.MaxFailedAccessAttempts = 5;
                options.Lockout.AllowedForNewUsers = true;

                // User settings
                options.User.RequireUniqueEmail = true;

                // SignIn settings
                options.SignIn.RequireConfirmedEmail = false;
                options.SignIn.RequireConfirmedPhoneNumber = false;
            });
           
            // Add CORS
            services.AddCors(options =>
            {
                options.AddPolicy("AllowAnyOrigin",
                    builder => builder
                        .AllowAnyOrigin()
                        .AllowAnyMethod()
                        .AllowAnyHeader());
            });

```

### Connection String With Database 
``` c#
  "ConnectionStrings": {
  
     // Name Servier And Name Database And Type Connection 
    "DefaultConnection": "Server=DESKTOP-P0JUNSB;Database=Movies;Trusted_Connection=True;Encrypt=false;"
    
  },
```
