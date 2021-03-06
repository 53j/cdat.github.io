---
title: CDMS Chapter 3
layout: docs
manual: cdms
index: 3
---







## CHAPTER 3  cdtime Module
 

<a name="3.1"></a>

#### 3.1 Time types

The `cdtime` module implements the CDMS time types, methods, and calendars. These are made available with the command

{% highlight python %}
import cdtime
{% endhighlight %}

Two time types are available: relative time and component time. Relative time is time relative to a fixed base time. It consists of:

 - a units string, of the form 'units since basetime', and
 - a floating-point value

For example, the time "28.0 days since 1996-1-1" has value=28.0, and units='days since 1996-1-1'

Component time consists of the integer fields year, month, day, hour, minute, and the floating-point field second. A sample component time is `1996-2-28 12:10:30.0`

The `cdtime` module contains functions for converting between these forms, based on the common calendars used in climate simulation. Basic arithmetic and comparison operators are also available.

<a name="3.2"></a>

#### 3.2 Calendars

A calendar specifies the number of days in each month, for a given year. cdtime supports these calendars:

  - `cdtime.GregorianCalendar`: years evenly divisible by four are leap years, except century years not evenly divisible by 400. This is sometimes called the proleptic Gregorian calendar, meaning that the algorithm for leap years applies for all years.
  - `cdtime.MixedCalendar`: mixed Julian/Gregorian calendar. Dates before 158210-15 are encoded with the Julian calendar, otherwise are encoded with the Gregorian calendar. The day immediately following 1582-10-4 is 1582-10-15. This is the default calendar.
  - `cdtime.JulianCalendar`: years evenly divisible by four are leap years,
  - `cdtime.NoLeapCalendar`: all years have 365 days,
  - `cdtime.Calendar360`: all months have 30 days.


Several `cdtime` functions have an optional calendar argument. The default calendar is the `MixedCalendar`. The default calendar may be changed with the command:

{% highlight python %}
cdtime.DefaultCalendar = newCalendar
{% endhighlight %}

<a name="3.3"></a>

#### 3.3 Time Constructors

The following table describes the methods for creating time types.

<a name="table_3.1"></a>

###### Table 3.1 Time Constructors

<table class="table">
  <tr>
    <th>Type</th>

    <th>Constructor</th>

    <th>Definition</th>
  </tr>

  <tr>
    <td>Reltime</td>

    <td><code>cdtime.reltime(value, relunits)</code></td>

    <td>
      <p>Create a relative time type.</p>

      <p><code>value</code> is an integer or floating point value.</p>

      <p><code>relunits</code> is a string of the form "<i>unit(</i>s) [since
      <i>basetime</i>]" where</p>

      <p><code>unit = [second | minute | hour | day | week | month | season |
      year]</code></p>

      <p><code>basetime</code> has the form <code>yyyy-mm-dd hh:mi:ss</code>. The
      default basetime is 1979-1-1, if no <code>since</code> clause is specified.</p>

      <p><strong>Example:</strong></p>

      <p><code>r = cdtime.reltime(28, "days since 1996-1-1")</code></p>
    </td>
  </tr>

  <tr>
    <td>Comptime</td>

    <td><code>cdtime.comptime(year, month=1, day=1, hour=0, minute=0,
    second=0.0)</code></td>

    <td>
      <p>Create a component time type.</p>

      <p><code>year</code> is an integer.</p>

      <p><code>month</code> is an integer in the range 1 .. 12</p>

      <p><code>day</code> is an integer in the range 1 .. 31</p>

      <p><code>hour</code> is an integer in the range 0 .. 23</p>

      <p><code>minute</code> is an integer in the range 0 .. 59</p>

      <p><code>second</code> is a floating point number in the range 0.0 ,, 60.0</p>

      <p><strong>Example:</strong></p>

      <p><code>c = cdtime.comptime(1996, 2, 28)</code></p>
    </td>
  </tr>

  <tr>
    <td>Comptime</td>

    <td>
      <p><strong>[Deprecated]</strong> <code>cdtime.abstime(absvalue,
      absunits)</code></p>
    </td>

    <td>
      <p>Create a component time from an absolute time representation.</p>

      <p><code>absvalue</code> is a floating-point encoding of an absolute time.</p>

      <p><code>absunits</code> is the units template, a string of the form <span>"unit
      as</span> <span>format"</span>, where unit is one of <span>second, minute, hour,
      day,</span> <span>calendar_month,</span> or <span>calendar_year.</span> format is
      a string ofthe form <span>"%x[%x[...] ][.%f]"</span>, where <span>'x'</span> is
      one of the formatletters <span>'Y'</span> (year, including century),
      <span>'m'</span> (two digit month,01=January), <span>'d'</span> (two-digit day
      within month), <span>'H'</span> (hourssince midnight), <span>'M'</span>
      (minutes), or <span>'S'</span> (seconds ). The optional <span>'.%f'</span>
      denotes a floating-point fraction of the unit.</p>

      <p><strong>Example:</strong></p>

      <p><code>c = cdtime.abstime(19960228.0, "day as %Y%m%d.%f")</code></p>
    </td>
  </tr>
</table>


<a name="3.4"></a>

#### 3.4 Relative Time


A relative time type has two members, value and units. Both can be set.


<a name="table_3.2"></a>

###### Table 3.2 Relative Time Members



|Type|Name|Summary|
|---|---|---|
|Float|value|Number of units|
|String|units|Relative units, of the form "unit(s) since  basetime|


<a name="3.5"></a>

#### 3.5 Component Time

A component time type has six members, all of which are settable.

<a name="table_3.3"></a>

###### Table 3.3 Component Time Membersch3\_cdms\_4.0.html/#Table\_3.1


|Type|Name|Summary|
|---|---|---|
|Integer|year|Year value|
|Integer|month|Month, in the range 1..12|
|Integer|day|Day of month, in the range 1 .. 31|
|Integer|hour|Hour, in the range 0 .. 23|
|Integer|minute|Minute, in the range 0 .. 59|
|Float|second|Seconds, in the range 0.0 .. 60.0|


<a name="3.6"></a>

#### 3.6 Time Methods

The following methods apply both to relative and component times.

<a name="table_3.4"></a>

###### Table 3.4 Time Methods

<table class="table">
  <tr>
    <th>Type</th>
    <th>Method</th>
    <th>Definition</th>
    <th>Examples</th>
  </tr>
  <tr>
    <td>Comptime or Reltime</td>
    <td><code>t.add(value, intervalUnits, calendar=cdtime.Default-Calendar)</code></td>
    <td>
      <p>Add an interval of time to a time type t. Returns the same type of time.</p>
      <p> <code>value</code> is the Float number of interval units.</p>
      <p> <code>intervalUnits</code> is </p> <pre style="word-break:normal;">cdtime.[Second(s) | Minute(s) | Hour(s) | Day(s) | Week(s) | Month(s) | Season(s) | Year(s)]</pre>
      <p> <code>calendar</code> is the calendar type.</p>
    </td>
    <td>
      <pre style="word-break:normal;">
>>> from cdtime import *
>>> c = comptime(1996,2,28)
>>> r = reltime(28,"days since 1996-1-1")
>>> print r.add(1,Day)
29.00 days since 1996-1-1
>>> print c.add(36,Hours)
1996-2-29 12:0:0.0</pre>
      <p><strong>Note:</strong> When adding or subtracting intervals of months or years, only the month and year of the result are significant. The reason is that intervals in months/years are not commensurate with intervals in days or fractional days. This leads to results that may be surprising. For example:</p>
      <pre style="word-break:normal;">
>>> c = comptime(1979,8,31)
>>> c.add(1,Month)
1979-9-1 0:0:0.0</pre>
      <p>In other words, the day component of c was ignored in the addition, and the day/hour/minute components of the results are just the defaults. If the interval is in years, the interval is converted internally to months:</p>
      <pre style="word-break:normal">
>>> c = comptime(1979,8,31)
>>> c.add(2,Years)
1981-8-1 0:0:0.0</pre>
    </td>
  </tr>
  <tr>
    <td>Integer</td>
    <td><code>t.cmp(t2, calendar=cdtime.DefaultCalendar)</code></td>
    <td>
      <p>Compare time values t and t2. Returns -1, 0, 1 as t is less than,
equal to, or greater than t2 respectively.</p>
      <p> <code>t2</code> is the time to compare.</p>
      <p> <code>calendar</code> is the calendar type.</p>
    </td>
    <td> 
      <pre style="word-break:normal;">>>> from cdtime import *
>>> r = cdtime.reltime(28,"days since 1996-1-1")
>>> c = comptime(1996,2,28)
>>> print r.cmp(c)
-1
>>> print c.cmp(r)
1
>>> print r.cmp(r)
0</pre>
    </td>
  </tr>
  <tr>
    <td>Comptime or Reltime</td>
    <td><code>t.sub(value, intervalUnits, calendar=cdtime.DefaultCalendar)</code></td>
    <td>
      <p>Subtract an interval of time from a time type t. Returns the same type of time.</p>
      <p><code>value</code> is the Float number of interval units.</p>
      <p>  <code>intervalUnits</code> is </p> <pre style="word-break:normal;">cdtime.[Second(s) | Minute(s) | Hour(s) | Day(s) | Week(s) | Month(s) | Season(s) | Year(s)]</pre>
      <p><code>calendar</code> is the calendar type.</p>
    </td>
    <td><pre style="word-break:normal;">>>> from cdtime import *
>>> r = cdtime.reltime(28,"days since 1996-1-1")
>>> c = comptime(1996,2,28)
>>> print r.sub(10,Days)
18.00 days since 1996-1-1
>>> print c.sub(30,Days)
1996-1-29 0:0:0.0</pre>
      <p>For intervals of years or months, see the note under add().</p>
    </td>
  </tr>
  <tr>
    <td>Comptime</td>
    <td><code>t.tocomp(calendar = cdtime.DefaultCalendar)</code></td>
    <td>
      <p>Convert to component time. Returns the equivalent component time.</p>
      <p><code>calendar</code> is the calendar type.</p>
    </td>
    <td>
      <pre style="word-break:normal;">>>> r = cdtime.reltime(28,"days since 1996-1-1")
>>> r.tocomp()
1996-1-29 0:0:0.0</pre>
    </td>
  </tr>
  <tr>
    <td>Reltime</td>
    <td><code>t.torel(units, calendar=cdtime.DefaultCalendar)</code></td>
    <td>Convert to relative time. Returns the equivalent relative time.</td>
    <td>
      <pre style="word-break:normal;">>>> c = comptime(1996,2,28)
>>> print c.torel("days since 1996-1-1")
58.00 days since 1996-1-1
>>> r = reltime(28,"days since 1996-1-1")
>>> print r.torel("days since 1995")
393.00 days since 1995
>>> print r.torel("days since 1995").value
393.0</pre>
    </td>
  </tr>
</table>


