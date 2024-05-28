# Anomaly 12-15 - While My WAF Gently Weeps - All Parts

## Preface
I am loading these logs into excel but you can use any snort log tool of your choosing.

## Anomaly 12
One of the coloumbs has this interesting alert attached to it. Sorting by that revals a couple ip addresses
![image_14.png](image_14.png)
Sorting and removing dupicates plus combing leaves me with
![image_15.png](image_15.png)

## Anomaly 13
So we know the return method was a 200 se lets sort by that first. Then we see that it is using a site called MeterWise analytics lets try filtering that down
![image_16.png](image_16.png)
My coloumb i filter for Meterwise analystics and see a few options but one stands out. Having to readd the one to the user agent gets us the flag

## Anomaly 14
So we need to filter bassed on sql injection so I am looking for the word OR in the uri
![image_17.png](image_17.png)
There is the latest one excel, have to put in other time format

## Anomaly 15
We are looking for path transversal so we need to filter based on `../`
![image_18.png](image_18.png)
One line doesnt have a snort alert attached to it