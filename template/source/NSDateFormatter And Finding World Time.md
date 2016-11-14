title: NSDateFormatter And Finding World Time
date: 2014-12-03 18:00:00 +0800
author: me
draft: false #草稿，可选
top: false #置顶文章，可选
preview:  When I was writing an app of finding world time, I find a really interesting issue of `NSDateFormatter`.When we initialize a `NSDate` object, with `[NSDate date]`, it’s GMT time, rather than the time depends on our local time zone; and when we wants to print the string of local time by using `NSDateFormatter` it will add the time difference between tour local time zone and the GMT time directly.
tags: #可选
    - Objective-c
    - iOS

---

When I was writing an app of finding world time, I find a really interesting issue of `NSDateFormatter`.

When we initialize a `NSDate` object, with `[NSDate date]`, it’s GMT time, rather than the time depends on our local time zone; and when we wants to print the string of local time by using `NSDateFormatter` it will add the time difference between tour local time zone and the GMT time directly.

Now we are going to find another city’s time. First we will initialize `NSDate` with GMT time. Then we use the `NSTimeZone` to find the time zone of the city which you want to know its time( `citiesTimeZone`), and the time zone of the city your location now( `locolTimeZone`).

Then, we use NSTimeInterval to find time differences between your location and GMT time( `locolTimeInterval`) and between the city you pick and GMT time(`citysecondFromGMT`).

We know if we want to know the time of city we pick, we just need to add time difference from GMT time( `citysecondFromGMT`). Why we need to find the locolTimeZone？

As I said before, when we use NSDateFormatter, it will add the time difference between tour local time zone and the GMT time(`locolTimeInterval`) directly. So, we need to minus this `locolTimeInterval` from `citysecondFromGMT`.

After all, we can return a weird date, `NSDateFormatter` this date, and then you can print the right thing you want.

~~~ objective-c
- (NSDate *)transferTimeBy:(NSString *)cityTimeZoneName {
	//现在的绝对时间，是以GMT时区来表示的
	NSDate *GMTDate=[NSDate date];

	//timezone you pick
	NSTimeZone *citiesTimeZone=[NSTimeZone 	timeZoneWithName:cityTimeZoneName];
	//所选城市所在时区距离GMT所差的时间（in second）
	NSTimeInterval citysecondFromGMT = [citiesTimeZone secondsFromGMT];

	//locol time zone
	NSTimeZone *locolTimeZone = [NSTimeZone localTimeZone];
	//本地时区距离GMT所差的时间
	NSTimeInterval locolTimeInterval = [locolTimeZone secondsFromGMT];
	//在NSDateFormatter中，NSDateFormatter为了把GMT时间正确输出为所在地时区的时间，会加上本地时区距离GMT所差的时间（locolTimeInterval），所以我们转换时区是需要把它从所选时区距离GMT时间时差中减掉
	citysecondFromGMT = citysecondFromGMT - locolTimeInterval;
	GMTDate = [NSDate dateWithTimeInterval:citysecondFromGMT 	sinceDate:GMTDate];
	return GMTDate;
}
~~~
