# Cron Based Background Services in .Net Core 3
## Providing recurrent scoped services.

Utilises [NCrontab](https://github.com/atifaziz/NCrontab) for underlying Cron support.

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using Prom3theu5.CronBasedHostedServices;

namespace Example
{
    public class Worker : ScheduledProcessor
    {
        protected override string Schedule => CronTab.Minutely();

        public Worker(IServiceScopeFactory serviceScopeFactory) : base(serviceScopeFactory)
        {
        }

        public override Task ProcessInScope(IServiceProvider serviceProvider)
        {
            using (var scope = serviceProvider.CreateScope())
            {
                ILogger<Worker> logger = scope.ServiceProvider.GetRequiredService <ILogger<Worker>>();
                logger.LogInformation("Worker running at: {time}", DateTimeOffset.Now);
            }

            return Task.CompletedTask;
        }
    }
}
````
