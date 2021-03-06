# Changes in D3 4.0

N.B.: This document is a work-in-progress. It does not yet cover all API changes.

## Modularity

D3 3.x was a monolithic library: the core functionality resided in a single [repository](https://github.com/d3/d3) and was published in a [single file](https://d3js.org/d3.v3.js). It was possible to create a custom build using a [nonstandard tool](https://github.com/mbostock/smash), but not easy, and few did. (There were also plugins, but these could only add features, and had their own [monolithic repository](https://github.com/d3/d3-plugins).)

D3 4.0 is modular. Instead of one library, D3 now consists of [many small libraries](https://github.com/d3) that are designed to work together, but are not required to; you can pick and choose which parts to use as you see fit. Each library is maintained in a separate repository, allowing decentralized ownership and independent release cycles. Want to own a new repository in the [D3 organization](https://github.com/d3)? [Let me know!](https://twitter.com/mbostock)

The [default bundle](https://d3js.org/d3.v4.0.0-alpha.45.js) of D3 4.0 conveniently aggregates [about thirty](https://github.com/d3/d3/blob/master/index.js) of these microlibraries.

```html
<script src="https://d3js.org/d3.v4.0.0-alpha.45.js"></script>
<script>

d3.select("body")
  .append("p")
    .text("Hello, world!");

</script>
```

But you don’t have to use the default bundle. Custom bundles are useful for applications that use only a subset of D3’s features; for example, a React charting library might use D3’s scales and shapes, but use React instead of selections to manipulate the DOM. You can load D3 microlibraries using vanilla script tags or RequireJS (great for HTTP/2!). For example, if you’re just using d3-selection, load it directly (5KB) instead of loading all of D3 (64KB):

```html
<script src="https://d3js.org/d3-selection.v0.8.min.js"></script>
<script>

d3.select("body")
  .append("p")
    .text("Hello, world!");

</script>
```

You can also `cat` D3 microlibraries into a custom bundle, or use tools such as [Webpack](https://webpack.github.io/) or [Rollup](http://rollupjs.org/) to create [optimized bundles](https://bl.ocks.org/mbostock/bb09af4c39c79cffcde4). I recommend Rollup: the D3 microlibraries are written as [ES6 modules](http://www.2ality.com/2014/09/es6-modules-final.html), and Rollup lets you pick at the symbol level to produce the smallest bundles!

Small files are nice, but modularity is also about making D3 *fun* again. Microlibraries are easier to understand, develop and test. They make it easier for new people to get involved and contribute. I want to reduce the distinction between a “core module” and a “plugin”, and increase the pace of development in D3 features.

If you don’t care about modularity, you can mostly ignore this change and keep using the default bundle. However, there’s an unavoidable consequence of adopting ES6 modules: every symbol in D3 4.0 now shares a flat namespace rather than the nesting employed in D3 3.x. For example, d3.scale.linear is now d3.scaleLinear, and d3.layout.treemap is now d3.treemap. There have also been significant changes (hopefully improvements) to D3’s features! Rather than list all the renamed symbols here, I’ll cover the changes in the sections below.

## Coding Style

No longer requires UTF-8 delivery: only ASCII is used. τ is dead; long live tau. Greek readers must make do with English transliteration.

The non-minified library is no longer mangled by UglifyJS, making it much more readable, and preserving inline comments. The use of ES6 modules and [magic-string](https://github.com/Rich-Harris/magic-string) also improves readability. Not to mention nearly all of the code has been rewritten in a cleaner style. There is almost no code in D3 3.x that remains in D3 4.0.

I’m not currently generating source maps, but I probably should.

## d3-array

* d3.layout.histogram ↦ d3.histogram; redesign histogram API
* change d3.range
* change accessor callback pattern
* add d3.scan
* add d3.ticks
* add d3.tickStep

## d3-axis

* d3.svg.axis, *axis*.orient ↦ d3.axisTop, d3.axisRight, d3.axisBottom, d3.axisLeft
* add *axis*.tickArguments
* *axis*.tickSize now only takes one argument
* *axis*.innerTickSize ↦ *axis*.tickSizeInner
* *axis*.outerTickSize ↦ *axis*.tickSizeOuter
* improve the default appearance of the axis

## d3-brush

* d3.svg.brush, *brush*.x, *brush*.y ↦ d3.brush, d3.brushX, d3.brushY
* *brush*.event ↦ *brush*.move
* *brushstart* event ↦ *start* event
* *brushend* event ↦ *end* event
* add *brush*.handleSize
* add *brush*.filter; ignore right-click by default
* improve the default appearance of the brush
* improve brush interaction (e.g., SHIFT key)
* brushes no longer use scales; they operate in screen coordinates
* brushes no longer store state internally; it is stored on applied elements
* remove *brush*.clamp; always clamps to the brushable region
* consume handled events

## d3-chord

* d3.svg.chord ↦ d3.ribbon (TODO)
* d3.layout.chord ↦ d3.chord (TODO)

## d3-collection

## d3-color

## d3-dsv

* d3.csv.parse ↦ d3.csvParse
* d3.csv.parseRows ↦ d3.csvParseRows
* d3.csv.format ↦ d3.csvFormat
* d3.csv.formatRows ↦ d3.csvFormatRows
* d3.tsv.parse ↦ d3.tsvParse
* d3.tsv.parseRows ↦ d3.tsvParseRows
* d3.tsv.format ↦ d3.tsvFormat
* d3.tsv.formatRows ↦ d3.tsvFormatRows

## d3-drag

* d3.behavior.drag ↦ d3.drag

## d3-ease

## d3-force

* d3.layout.force ↦ d3.forceSimulation

## d3-geo

* d3.geo.graticule ↦ d3.geoGraticule
* d3.geo.circle ↦ d3.geoCircle
* d3.geo.area ↦ d3.geoArea
* d3.geo.bounds ↦ d3.geoBounds
* d3.geo.centroid ↦ d3.geoCentroid
* d3.geo.distance ↦ d3.geoDistance
* d3.geo.interpolate ↦ d3.geoInterpolate
* d3.geo.length ↦ d3.geoLength
* d3.geo.rotation ↦ d3.geoRotation
* d3.geo.stream ↦ d3.geoStream

## d3-geo-projection

* d3.geo.path ↦ d3.geoPath

## d3-hierarchy

* d3.layout.cluster ↦ d3.cluster
* d3.layout.hierarchy ↦ d3.hierarchy
* d3.layout.pack ↦ d3.pack
* d3.layout.partition ↦ d3.partition
* d3.layout.tree ↦ d3.tree
* d3.layout.treemap ↦ d3.treemap

## d3-interpolate

* d3.interpolators ↦ REMOVED

## d3-format

## d3-path

## d3-polygon

* d3.geom.polygon.area ↦ d3.polygonArea
* d3.geom.polygon.centroid ↦ d3.polygonCentroid
* d3.geom.hull ↦ d3.polygonHull

## d3-quadtree

* d3.geom.quadtree ↦ d3.quadtree

## d3-random

* d3.random.normal ↦ d3.randomNormal
* d3.random.logNormal ↦ d3.randomLogNormal
* d3.random.bates ↦ d3.randomBates
* d3.random.irwinHall ↦ d3.randomIrwinHall

## d3-request

* d3.xhr ↦ d3.request

## d3-scale

* d3.scale.linear ↦ d3.scaleLinear
* d3.scale.sqrt ↦ d3.scaleSqrt
* d3.scale.pow ↦ d3.scalePow
* d3.scale.log ↦ d3.scaleLog
* d3.scale.quantize ↦ d3.scaleQuantize
* d3.scale.threshold ↦ d3.scaleThreshold
* d3.scale.quantile ↦ d3.scaleQuantile
* d3.scale.identity ↦ d3.scaleIdentity
* d3.scale.ordinal ↦ d3.scaleOrdinal
* d3.scale.category10 ↦ d3.schemeCategory10
* d3.scale.category20 ↦ d3.schemeCategory20
* d3.scale.category20b ↦ d3.schemeCategory20b
* d3.scale.category20c ↦ d3.schemeCategory20c
* d3.time.scale ↦ d3.scaleTime

## d3-selection

## d3-shape

* d3.svg.line ↦ d3.line
* d3.svg.line.radial ↦ d3.radialLine
* d3.svg.area ↦ d3.area
* d3.svg.area.radial ↦ d3.radialArea
* d3.svg.arc ↦ d3.arc
* d3.svg.symbol ↦ d3.symbol
* d3.svg.symbolTypes ↦ d3.symbolTypes
* d3.svg.diagonal ↦ REMOVED
* d3.svg.diagonal.radial ↦ REMOVED
* d3.layout.bundle ↦ d3.curveBundle
* d3.layout.stack ↦ d3.stack

## d3-time-format

* d3.time.format ↦ d3.timeFormat
* d3.time.format.multi ↦ REMOVED
* d3.time.format.utc ↦ d3.utcFormat
* d3.time.format.iso ↦ d3.isoFormat

## d3-time

* d3.time.interval ↦ d3.timeInterval
* d3.time.day ↦ d3.timeDay
* d3.time.days ↦ d3.timeDays
* d3.time.dayOfYear ↦ d3.timeDay.count
* d3.time.hour ↦ d3.timeHour
* d3.time.hours ↦ d3.timeHours
* d3.time.minute ↦ d3.timeMinute
* d3.time.minutes ↦ d3.timeMinutes
* d3.time.month ↦ d3.timeMonth
* d3.time.months ↦ d3.timeMonths
* d3.time.second ↦ d3.timeSecond
* d3.time.seconds ↦ d3.timeSeconds
* d3.time.sunday ↦ d3.timeSunday
* d3.time.sundays ↦ d3.timeSundays
* d3.time.sundayOfYear ↦ d3.timeSunday.count
* d3.time.monday ↦ d3.timeMonday
* d3.time.mondays ↦ d3.timeMondays
* d3.time.mondayOfYear ↦ d3.timeMonday.count
* d3.time.tuesday ↦ d3.timeTuesday
* d3.time.tuesdays ↦ d3.timeTuesdays
* d3.time.tuesdayOfYear ↦ d3.timeTuesday.count
* d3.time.wednesday ↦ d3.timeWednesday
* d3.time.wednesdays ↦ d3.timeWednesdays
* d3.time.wednesdayOfYear ↦ d3.timeWednesday.count
* d3.time.thursday ↦ d3.timeThursday
* d3.time.thursdays ↦ d3.timeThursdays
* d3.time.thursdayOfYear ↦ d3.timeThursday.count
* d3.time.friday ↦ d3.timeFriday
* d3.time.fridays ↦ d3.timeFridays
* d3.time.fridayOfYear ↦ d3.timeFriday.count
* d3.time.saturday ↦ d3.timeSaturday
* d3.time.saturdays ↦ d3.timeSaturdays
* d3.time.saturdayOfYear ↦ d3.timeSaturday.count
* d3.time.week ↦ d3.timeWeek
* d3.time.weeks ↦ d3.timeWeeks
* d3.time.weekOfYear ↦ d3.timeWeek.count
* d3.time.year ↦ d3.timeYear
* d3.time.years ↦ d3.timeYears
* d3.time.day.utc ↦ d3.utcDay
* d3.time.days.utc ↦ d3.utcDays
* d3.time.dayOfYear.utc ↦ d3.utcDay.count
* d3.time.hour.utc ↦ d3.utcHour
* d3.time.hours.utc ↦ d3.utcHours
* d3.time.minute.utc ↦ d3.utcMinute
* d3.time.minutes.utc ↦ d3.utcMinutes
* d3.time.month.utc ↦ d3.utcMonth
* d3.time.months.utc ↦ d3.utcMonths
* d3.time.second.utc ↦ d3.utcSecond
* d3.time.seconds.utc ↦ d3.utcSeconds
* d3.time.sunday.utc ↦ d3.utcSunday
* d3.time.sundays.utc ↦ d3.utcSundays
* d3.time.sundayOfYear.utc ↦ d3.utcSunday.count
* d3.time.monday.utc ↦ d3.utcMonday
* d3.time.mondays.utc ↦ d3.utcMondays
* d3.time.mondayOfYear.utc ↦ d3.utcMonday.count
* d3.time.tuesday.utc ↦ d3.utcTuesday
* d3.time.tuesdays.utc ↦ d3.utcTuesdays
* d3.time.tuesdayOfYear.utc ↦ d3.utcTuesday.count
* d3.time.wednesday.utc ↦ d3.utcWednesday
* d3.time.wednesdays.utc ↦ d3.utcWednesdays
* d3.time.wednesdayOfYear.utc ↦ d3.utcWednesday.count
* d3.time.thursday.utc ↦ d3.utcThursday
* d3.time.thursdays.utc ↦ d3.utcThursdays
* d3.time.thursdayOfYear.utc ↦ d3.utcThursday.count
* d3.time.friday.utc ↦ d3.utcFriday
* d3.time.fridays.utc ↦ d3.utcFridays
* d3.time.fridayOfYear.utc ↦ d3.utcFriday.count
* d3.time.saturday.utc ↦ d3.utcSaturday
* d3.time.saturdays.utc ↦ d3.utcSaturdays
* d3.time.saturdayOfYear.utc ↦ d3.utcSaturday.count
* d3.time.week.utc ↦ d3.utcWeek
* d3.time.weeks.utc ↦ d3.utcWeeks
* d3.time.weekOfYear.utc ↦ d3.utcWeek.count
* d3.time.year.utc ↦ d3.utcYear
* d3.time.years.utc ↦ d3.utcYears

## d3-timer

* d3.timer.flush ↦ d3.timerFlush

## d3-transition

## d3-voronoi

* d3.geom.voronoi ↦ d3.voronoi

## d3-zoom

* d3.behavior.zoom ↦ d3.zoom
