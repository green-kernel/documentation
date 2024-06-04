`powermetrics` is a tool on MacOS that can give you detailed usage statistics on a per process level. Unfortunately is not documented very well and there is a lot of mystery about it. But it is a great tool that can give you great insights about your energy impact. The important thing to note is that it is not the actual energy a process uses but more a hint and an indicator. 

For this project the thing we are interest is in how is the the energy impact value created and what values are used. This great [article](https://blog.mozilla.org/nnethercote/2015/08/26/what-does-the-os-x-activity-monitors-energy-impact-actually-measure/) goes into great detail and we can extract the following formular looking at the plist files.
It is important to note that energy impact has nothing to do with the actual energy used and that the values incorporates things like diskio or network with a penalty factor that varies from machine to machine. For the sake of simplicity we will use the `default.plist` for now. We also ignore the quality of service penalties as these are not available under Linux. Also there is a difference between network packages and bytes.

```
(cpu_time * penalty)  + (cpu_wakeups * penalty) + (disk_bytes_read * penalty) + (disk_bytes_write * penalty) + (gpu_time * penalty) + (network_rec_packets * penalty) + (netwokr_sent_packets * penality)
```

filling in all the penalty values from the plist file this gives us

```
(cpu_time * 1) + (cpu_wakeups * 2.0e-4) + (disk_bytes_read * 4.5e-10) + (disk_bytes_write * 2.4e-10) + (gpu_time * 0.0) + (network_rec_packets * 4.0e-6) + (netwokr_sent_packets * 4.0e-6)
```

So the values we need to get from the kernel are:
- cpu_time
- cpu_wakeups
- disk_bytes_read
- disk_bytes_write
- network_rec_packets
- netwokr_sent_packets

## Links
- A great little write up but not up to date anymore: https://firefox-source-docs.mozilla.org/performance/powermetrics.html
- https://blog.mozilla.org/nnethercote/2015/08/26/what-does-the-os-x-activity-monitors-energy-impact-actually-measure/
