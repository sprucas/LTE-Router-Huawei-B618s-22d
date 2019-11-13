# LTE-Router-Huawei-B618s-22d
bash scripts to query status information and reset the router
## Scripts
### getDynamicInfo
retrieves properties from the router, which change frequently (e.g. the number of received bytes)
the data is stored in XML files like this:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<response>
<CurrentConnectTime>1586</CurrentConnectTime>
<CurrentUpload>126997313</CurrentUpload>
<CurrentDownload>319100864</CurrentDownload>
<CurrentDownloadRate>52</CurrentDownloadRate>
<CurrentUploadRate>135</CurrentUploadRate>
<TotalUpload>11960851325</TotalUpload>
<TotalDownload>69832154477</TotalDownload>
<TotalConnectTime>377385</TotalConnectTime>
<showtraffic>1</showtraffic>
</response>
```
and also parsed into files which can be inclused in bash scripts:
```bash
TotalUpload=11960814672
TotalDownload=69832083539
CurrentConnectTime=1526
CurrentDownloadRate=0
CurrentUploadRate=0
rsrq=-9
rsrp=-88
rssi=-67
sinr=9
band=3
cell_id=35065491
```
### getStaticInfo
retrieves properties from the router, which change infrequently (e.g. the IMEI)
the data is stored in XML files like this:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<response>
<NetworkMode>03</NetworkMode>
<NetworkBand>3FFFFFFF</NetworkBand>
<LTEBand>800C5</LTEBand>
</response>
```
### restart
causes the router to perform a reset
## Setup
* set the IP address or name of the router in property `HOST` of file `base`
* set the directory, in which the files shall be stored in property `DIR` of file `base`

***use a dedicated directory for this, because its contents will be deleted!***
* set the name of the user, which shall be used to log into the router in property `USER` of file `login`
* set the password, which shall be used to log into the router in property `PASS` of file `login`
### Requirements
like most bash scripts, some tools are needed for these scripts:
* wget
* base64
* sha256sum
## Monitoring
script `pushLTEStatus`can be used to push the values to [Prometheus](https://prometheus.io/). It creates a `.prom` file which can be read by [node_exporter](https://prometheus.io/download/#node_exporter). node exported has to be started with the textfile collector enabled (e.g. `-collectors.enabled textfile -collector.textfile.directory /var/spool/prometheus`)

## NetMode
NetMode can be set py posting XML to /api/net/net-mode and will allow custom configuration of the Bands being used by the Modem

B618s-22d modem 
LTE: B1/3/7/8/20/38
FDD: 2100 MHz/1800 MHz/2600 MHz/900 MHz/800 MHz
TDD: 2600 MHz
Intra-band contiguous: CA_1C, CA_3C, CA_7C, CA_8B, CA_38C
Inter-band : CA_1A-3A, CA_1A-20A, CA_3A-7A, CA_3A-20A, CA_7A-20A,

B618s-65d modem
LTE: B1/3/5/7/8/28/40
FDD: 2100 MHz/1800 Mhz/850 MHz/900 MHz/700 MHz
TDD: 2600 MHz/2300 MHz
Intraband contiguous: CA_1C, CA_3C, CA_5B, CA_7C, CA_8B, CA_40C
Interband: CA_1A3A, CA_3A7A, CA_7A28A

Codes for each Band as follows
0000000001 B1 2100
0000000004 B3 1800
0000000040 B7 fdd 2600
0000000080 B8 900
0008000000 B28 700
8000000000 B40 2300

