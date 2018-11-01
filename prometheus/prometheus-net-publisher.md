# Prometheus scraping .NET console-App  

- Prometheus .NET client library: https://github.com/prometheus-net/prometheus-net
- Working silly console app publishing "gauge" data:
````
using System;
using System.Threading;
using System.Threading.Tasks;
using Prometheus;

namespace myConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                var metricServer = new MetricServer(port: 1234);
                metricServer.Start();

                CancellationTokenSource cts = new CancellationTokenSource();
                RunInBackground(cts);

                Console.WriteLine("Press a key to terminate...");
                Console.ReadKey();

                cts.Cancel();
                metricServer.Stop();
            }
            catch (Exception e)
            {
                Console.WriteLine(e);
            }
        }

        private static async void RunInBackground(CancellationTokenSource cts)
        {
            var token = cts.Token;
            await Task.Run(() =>
            {
                var gauge = Metrics.CreateGauge("gauge", "Ths is published from Mats' app.");
                var randomGenerator = new Random();

                while (!token.IsCancellationRequested)
                {
                    gauge.Inc(randomGenerator.NextDouble() * 10.0 - 5.0);
                    Console.WriteLine(gauge.Value);
                    Thread.Sleep(100);
                }
            });
        }
    }
}
````
This will publish to ````//localhost:1234/metrics````

- Add this console-app as target to prometheus-configuration (incl. in prometheus.yml to other config):
````
- job_name: mats
  scrape_interval: 5s
  static_configs:
  - targets:
    - localhost:1234
````

- If Prometheus is running in Docker-container (as part of docker-compose) on a dedicated network, prometheus can't reach localhost. 
Change all containers to use "host"-network (here as example in "docker-compose.yaml"):  
````
services:
  prometheus:
    network_mode: "host"
...
````

- Potentially also change other targets in prometheus-configs 
from ````{container-name}:{port}```` to ````localhost:{port}```` (as consequence to modified network-mode)
