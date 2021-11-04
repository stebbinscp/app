Big Data Homework 5

5-1
Hbase query:      get 'weather_delays_by_route_v0', 'LGAORD'

Hbase Output:
COLUMN                        CELL                                                                                                                              
delay:clear_delays            timestamp=1635557680484, value=937553                                                                                             
delay:clear_flights           timestamp=1635557680484, value=124376                                                                                             
delay:fog_delays              timestamp=1635557680484, value=491622                                                                                             
delay:fog_flights             timestamp=1635557680484, value=27828                                                                                              
delay:hail_delays             timestamp=1635557680484, value=4260                                                                                               
delay:hail_flights            timestamp=1635557680484, value=177                                                                                                
delay:rain_delays             timestamp=1635557680484, value=1455963                                                                                            
delay:rain_flights            timestamp=1635557680484, value=89125                                                                                              
delay:snow_delays             timestamp=1635557680484, value=273956                                                                                             
delay:snow_flights            timestamp=1635557680484, value=15107                                                                                              
delay:thunder_delays          timestamp=1635557680484, value=425870                                                                                             
delay:thunder_flights         timestamp=1635557680484, value=17071                                                                                              
delay:tornado_delays          timestamp=1635557680484, value=0                                                                                                  
delay:tornado_flights         timestamp=1635557680484, value=0

To determine the average flights, the math is delays/flights
Clear_delays/clear_flights	937553/124376 = 7.538
Fog_delays/fog_flights	491622/27828 = 17.667
Hail_delays/hail_flights	4260/177 = 24.67                                                                
Rain_delays/rain_flights	1455963/89125 = 16.337
Snow_delays/snow_flights	273956/15107 = 18.134
Thunder_delays/thunder_flights	425870/17071 = 24.950
Tornado_delays/tornado_flights	N/A



5-2

Hbase shell
Create ‘cstebbins_h252’, ‘delay’

create external table cstebbins_h252 (route string,clear_flights bigint, clear_delays bigint,fog_flights bigint, fog_delay bigint, rain_flights bigint, rain_delay bigint, snow_flights bigint, snow_delay bigint, hail_flights bigint, hail_delay bigint, thunder_flights bigint, thunder_delay bigint, tornado_flights bigint, tornado_delay bigint) STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler' WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,delay:clear_flights,delay:clear_delays,delay:fog_flights,delay:fog_delays,delay:rain_flights,delay:rain_delays,delay:snow_flights,delay:snow_delays,delay:hail_flights,delay:hail_delays,delay:thunder_flights,delay:thunder_delays,delay:tornado_flights,delay:tornado_delays') TBLPROPERTIES ('hbase.table.name' = 'cstebbins_h252');

insert overwrite table cstebbins_h252
select origin_name,
clear_flights, clear_delays,
fog_flights, fog_delays,
rain_flights, rain_delays,
snow_flights, snow_delays,
hail_flights, hail_delays,
thunder_flights, thunder_delays,
tornado_flights, tornado_delays
from route_delays;

testing url: http://ec2-52-14-115-151.us-east-2.compute.amazonaws.com:3050/route-delays.html

run command on cstebbins cluster: node app.js 3050 172.31.39.49 8070
Web app changes:
Changed table.css by changing the font to font-family:Monospace
Changed route-delays.html by removing the destination box
Removed destination from result.mustache
Changed table to cstebbins_h252, and remove references to destination

5-3

A scalable application needs to be persistent and the response to queries must be scalable.
Currently, we have a non persistent web application which likely queues queries. 
To fix this, we will need to first implement persistence, and then implement a scalable querying service for the application, perhaps via AWS API Gateway to load balance and scale should more queries be made.
