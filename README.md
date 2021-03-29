# ezdates.pl 100,000 line PERL for Date and calendar calculator script language with nested loops, dynamic runtime debugging, variables, flexibility, built-in regression testing sanity check, documentation, user manual, debugging tree hierarchy to trace the code execution path of procedures and sections, etc...

# QUICK REFERENCE:
# +===============================================================================================+
# | ezdates.pl [command ;] [command ;] ...                                                        |
# | [date yyyy-mm-dd[_hh:mi:ss]]                                                                  |
# | [today|now] [tomorrow] [yesterday] [started] [ended]                                          |
# | [format [long] [short] [date-time] [time-date] [date] [time]]                                 |
# | [format [default] [unix-date fmt-string] [refmis default]]                                    |
# | [print] [noprint] [print [blank] line] [print blank [line]]                                   |
# | [[read] modified|mtime|accessed|created [date time of the file] filename]                     |
# | [sleep N]                                                                                     |
# | [trunc] [ceil] [round]                                                                        |
# | [som] [eom]                                                                                   |
# | [sosm] [eosm]                                                                                 |
# | [sow] [eow]                                                                                   |
# | [sosw] [eosw]                                                                                 |
# | [soq] [eoq]                                                                                   |
# | [sosq] [eosq]                                                                                 |
# | [seoy] [seoq] [seom] [seow] [seod] [seoh] [seomi]                                             |
# | [soy] [eoy]                                                                                   |
# | [sosy] [eosy]  [mioy] [mioq] [miom] [miow] [miod] [mioh]                                      |
# | [dow] [doy] [dom] [doq]                                                                       |
# | [moq] [moy]                                                                                   |
# | [qoy]                                                                                         |
# | [hod] [how] [hom] [hoq] [hoy]                                                                 |
# | [loop N; [command;] [loop M;...;end;] end;]                                                   |
# | [abs [year] [quarter] [month] [week] [day] [hour] [minute] [second]]                          |
# | [N[th] to [last] sun|mon|tue|wed|thu|fri|sat [[of the] month|quarter|year]]                   |
# | [add [+|-] N [ss|mi|hh|day|dow]]                                                              |
# | [beg|end|sun|mon|... of N[th] [last] [full|partial] week of month|qtr|year]                   |
# | [[refmis] unix date 'fmt-string']                                                             |
# | [[refmis] [start|end] [anchor|drift] [semi] fiscal month [period]]                            |
# | [[refmis] [anchor|drift] [fiscal] [month] [wall|line] [calendar] [[no] [title|body] [alone]]] |
# | [[year] [N [x] M] [year] [calendar]]                                                          |
# | [[matrix] [horizontal] [[J [x] K] [months]] [[N|n] [x] M days] [cols] [cells] [calendar]]     |
# | [[matrix] [vertical] [[J [x] K] [months]] [N [x] [M|n] days] [rows] [cells] [calendar]]       |
# | [[set] [$var|$1] = [$var|$1-$N]]                                                              |
# | [#comment] [comment 'comment-token']                                                          |
# | [regression test | sanity check]                                                              |
# | [dbg [[+|-]procname()] [on|off|[+|-]N]]                                                       |
# | [help [ref]]                                                                                  |
# +===============================================================================================+

# SOME EXAMPLES:
#-----------------------------------------------------------------------------|
#
# * Calculate end-of-month, and the end-of-month for next month.
#   perl ezdates.pl date 2006-11-15; fmt short; eom; print; add(+1); eom; print;
#
# * Results:
#   2006-11-30
#   2006-12-31
#
#-----------------------------------------------------------------------------|
#
# * Store the month end dates for this quarter, then display them.
#   perl ezdates.pl fmt short; date 2006-10-31; soq; eom; $eom1=$1; add 1; eom; $eom2=$1; add 1; eom; $eom3=$1; $1=$eom1; print; $1=$eom2; print; $1=$eom3; print;
#
# * Results:
#   2006-10-31
#   2006-11-30
#   2006-12-31
#
#-----------------------------------------------------------------------------|
#
# * Display the next 7 days.
#   perl ezdates.pl format short; date 2006-12-25; add +1; loop 7; print; add +1; end; noprint;
#
# * Results:
#   2006-12-26
#   2006-12-27
#   2006-12-28
#   2006-12-29
#   2006-12-30
#   2006-12-31
#
#-----------------------------------------------------------------------------|
#
# * Display a sequence of calendar months starting in a previous or last month and printing a blank line between each matrix calendar.
#   perl ezdates.pl date 2021-01-26; loop 1; start of month; -1; end; loop 6; print blank line; calendar; print; end-of-month; add +1; end; noprint;
#
# * Results:
#
#         2020-12
#   Su Mo Tu We Th Fr Sa
#         01 02 03 04 05
#   06 07 08 09 10 11 12
#   13 14 15 16 17 18 19
#   20 21 22 23 24 25 26
#   27 28 29 30 31
#
#         2021-01
#   Su Mo Tu We Th Fr Sa
#                  01 02
#   03 04 05 06 07 08 09
#   10 11 12 13 14 15 16
#   17 18 19 20 21 22 23
#   24 25 26 27 28 29 30
#   31
#
#         2021-02
#   Su Mo Tu We Th Fr Sa
#      01 02 03 04 05 06
#   07 08 09 10 11 12 13
#   14 15 16 17 18 19 20
#   21 22 23 24 25 26 27
#   28
#
#         2021-03
#   Su Mo Tu We Th Fr Sa
#      01 02 03 04 05 06
#   07 08 09 10 11 12 13
#   14 15 16 17 18 19 20
#   21 22 23 24 25 26 27
#   28 29 30 31
#
#         2021-04
#   Su Mo Tu We Th Fr Sa
#               01 02 03
#   04 05 06 07 08 09 10
#   11 12 13 14 15 16 17
#   18 19 20 21 22 23 24
#   25 26 27 28 29 30
#
#         2021-05
#   Su Mo Tu We Th Fr Sa
#                     01
#   02 03 04 05 06 07 08
#   09 10 11 12 13 14 15
#   16 17 18 19 20 21 22
#   23 24 25 26 27 28 29
#   30 31
#
#-----------------------------------------------------------------------------|
#
# * Display a sequence of calendar months in single line calendar type instead of wall matrix calendar type, starting in a previous month and printing a blank line between each line calendar.
#   perl ezdates.pl date 2021-01-26; loop 1; start of month; -1; end; loop 12; print blank line; line calendar; print; end-of-month; add +1; end; noprint;
#
# * Results:
#
#            |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa |  Su Mo  |
#   2020-12  |        01 02 03 04 05  |  06 07 08 09 10 11 12  |  13 14 15 16 17 18 19  |  20 21 22 23 24 25 26  |  27 28 29 30 31       |         |
#
#            |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa |  Su Mo  |
#   2021-01  |                 01 02  |  03 04 05 06 07 08 09  |  10 11 12 13 14 15 16  |  17 18 19 20 21 22 23  |  24 25 26 27 28 29 30 |  31     |
#
#            |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa |  Su Mo  |
#   2021-02  |     01 02 03 04 05 06  |  07 08 09 10 11 12 13  |  14 15 16 17 18 19 20  |  21 22 23 24 25 26 27  |  28                   |         |
#
#            |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa |  Su Mo  |
#   2021-03  |     01 02 03 04 05 06  |  07 08 09 10 11 12 13  |  14 15 16 17 18 19 20  |  21 22 23 24 25 26 27  |  28 29 30 31          |         |
#
#            |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa |  Su Mo  |
#   2021-04  |              01 02 03  |  04 05 06 07 08 09 10  |  11 12 13 14 15 16 17  |  18 19 20 21 22 23 24  |  25 26 27 28 29 30    |         |
#
#            |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa |  Su Mo  |
#   2021-05  |                    01  |  02 03 04 05 06 07 08  |  09 10 11 12 13 14 15  |  16 17 18 19 20 21 22  |  23 24 25 26 27 28 29 |  30 31  |
#
#            |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa |  Su Mo  |
#   2021-06  |        01 02 03 04 05  |  06 07 08 09 10 11 12  |  13 14 15 16 17 18 19  |  20 21 22 23 24 25 26  |  27 28 29 30          |         |
#
#            |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa |  Su Mo  |
#   2021-07  |              01 02 03  |  04 05 06 07 08 09 10  |  11 12 13 14 15 16 17  |  18 19 20 21 22 23 24  |  25 26 27 28 29 30 31 |         |
#
#            |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa |  Su Mo  |
#   2021-08  |  01 02 03 04 05 06 07  |  08 09 10 11 12 13 14  |  15 16 17 18 19 20 21  |  22 23 24 25 26 27 28  |  29 30 31             |         |
#
#            |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa |  Su Mo  |
#   2021-09  |           01 02 03 04  |  05 06 07 08 09 10 11  |  12 13 14 15 16 17 18  |  19 20 21 22 23 24 25  |  26 27 28 29 30       |         |
#
#            |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa |  Su Mo  |
#   2021-10  |                 01 02  |  03 04 05 06 07 08 09  |  10 11 12 13 14 15 16  |  17 18 19 20 21 22 23  |  24 25 26 27 28 29 30 |  31     |
#
#            |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa |  Su Mo  |
#   2021-11  |     01 02 03 04 05 06  |  07 08 09 10 11 12 13  |  14 15 16 17 18 19 20  |  21 22 23 24 25 26 27  |  28 29 30             |         |
#
#-----------------------------------------------------------------------------|
#
# * Display a sequence of calendar months in single line calendar type instead of wall matrix calendar type, starting in a previous month and printing a blank line between each matrix calendar.
#   perl ezdates.pl date 2021-01-26; loop 1; start of month; -1; end; line calendar title; print; print blank line; loop 12; line calendar body; print; end-of-month; add +1; end; noprint;
#
# * Results:
#            |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo Tu We Th Fr Sa  |  Su Mo  |
#
#   2020-12  |        01 02 03 04 05  |  06 07 08 09 10 11 12  |  13 14 15 16 17 18 19  |  20 21 22 23 24 25 26  |  27 28 29 30 31        |         |
#   2021-01  |                 01 02  |  03 04 05 06 07 08 09  |  10 11 12 13 14 15 16  |  17 18 19 20 21 22 23  |  24 25 26 27 28 29 30  |  31     |
#   2021-02  |     01 02 03 04 05 06  |  07 08 09 10 11 12 13  |  14 15 16 17 18 19 20  |  21 22 23 24 25 26 27  |  28                    |         |
#   2021-03  |     01 02 03 04 05 06  |  07 08 09 10 11 12 13  |  14 15 16 17 18 19 20  |  21 22 23 24 25 26 27  |  28 29 30 31           |         |
#   2021-04  |              01 02 03  |  04 05 06 07 08 09 10  |  11 12 13 14 15 16 17  |  18 19 20 21 22 23 24  |  25 26 27 28 29 30     |         |
#   2021-05  |                    01  |  02 03 04 05 06 07 08  |  09 10 11 12 13 14 15  |  16 17 18 19 20 21 22  |  23 24 25 26 27 28 29  |  30 31  |
#   2021-06  |        01 02 03 04 05  |  06 07 08 09 10 11 12  |  13 14 15 16 17 18 19  |  20 21 22 23 24 25 26  |  27 28 29 30           |         |
#   2021-07  |              01 02 03  |  04 05 06 07 08 09 10  |  11 12 13 14 15 16 17  |  18 19 20 21 22 23 24  |  25 26 27 28 29 30 31  |         |
#   2021-08  |  01 02 03 04 05 06 07  |  08 09 10 11 12 13 14  |  15 16 17 18 19 20 21  |  22 23 24 25 26 27 28  |  29 30 31              |         |
#   2021-09  |           01 02 03 04  |  05 06 07 08 09 10 11  |  12 13 14 15 16 17 18  |  19 20 21 22 23 24 25  |  26 27 28 29 30        |         |
#   2021-10  |                 01 02  |  03 04 05 06 07 08 09  |  10 11 12 13 14 15 16  |  17 18 19 20 21 22 23  |  24 25 26 27 28 29 30  |  31     |
#   2021-11  |     01 02 03 04 05 06  |  07 08 09 10 11 12 13  |  14 15 16 17 18 19 20  |  21 22 23 24 25 26 27  |  28 29 30              |         |
#
#-----------------------------------------------------------------------------|
#
# * Display custom number of days column, 5 columns, in a matrix of months;
#   perl ezdates.pl date 2021-01-01; matrix 4 x 3 month n x 5 days calendar;
#
# * Results:
#   |--------------------------------------------------------|
#   |     2021-01      |     2021-02      |     2021-03      |
#   |  Su Mo Tu We Th  |  Su Mo Tu We Th  |  Su Mo Tu We Th  |
#   |                  |     01 02 03 04  |     01 02 03 04  |
#   |  01 02 03 04 05  |  05 06 07 08 09  |  05 06 07 08 09  |
#   |  06 07 08 09 10  |  10 11 12 13 14  |  10 11 12 13 14  |
#   |  11 12 13 14 15  |  15 16 17 18 19  |  15 16 17 18 19  |
#   |  16 17 18 19 20  |  20 21 22 23 24  |  20 21 22 23 24  |
#   |  21 22 23 24 25  |  25 26 27 28     |  25 26 27 28 29  |
#   |  26 27 28 29 30  |                  |  30 31           |
#   |  31              |                  |                  |
#   |--------------------------------------------------------|
#   |     2021-04      |     2021-05      |     2021-06      |
#   |  Su Mo Tu We Th  |  Su Mo Tu We Th  |  Su Mo Tu We Th  |
#   |              01  |                  |        01 02 03  |
#   |  02 03 04 05 06  |     01 02 03 04  |  04 05 06 07 08  |
#   |  07 08 09 10 11  |  05 06 07 08 09  |  09 10 11 12 13  |
#   |  12 13 14 15 16  |  10 11 12 13 14  |  14 15 16 17 18  |
#   |  17 18 19 20 21  |  15 16 17 18 19  |  19 20 21 22 23  |
#   |  22 23 24 25 26  |  20 21 22 23 24  |  24 25 26 27 28  |
#   |  27 28 29 30     |  25 26 27 28 29  |  29 30           |
#   |                  |  30 31           |                  |
#   |--------------------------------------------------------|
#   |     2021-07      |     2021-08      |     2021-09      |
#   |  Su Mo Tu We Th  |  Su Mo Tu We Th  |  Su Mo Tu We Th  |
#   |              01  |  01 02 03 04 05  |           01 02  |
#   |  02 03 04 05 06  |  06 07 08 09 10  |  03 04 05 06 07  |
#   |  07 08 09 10 11  |  11 12 13 14 15  |  08 09 10 11 12  |
#   |  12 13 14 15 16  |  16 17 18 19 20  |  13 14 15 16 17  |
#   |  17 18 19 20 21  |  21 22 23 24 25  |  18 19 20 21 22  |
#   |  22 23 24 25 26  |  26 27 28 29 30  |  23 24 25 26 27  |
#   |  27 28 29 30 31  |  31              |  28 29 30        |
#   |                  |                  |                  |
#   |--------------------------------------------------------|
#   |     2021-10      |     2021-11      |     2021-12      |
#   |  Su Mo Tu We Th  |  Su Mo Tu We Th  |  Su Mo Tu We Th  |
#   |                  |     01 02 03 04  |           01 02  |
#   |  01 02 03 04 05  |  05 06 07 08 09  |  03 04 05 06 07  |
#   |  06 07 08 09 10  |  10 11 12 13 14  |  08 09 10 11 12  |
#   |  11 12 13 14 15  |  15 16 17 18 19  |  13 14 15 16 17  |
#   |  16 17 18 19 20  |  20 21 22 23 24  |  18 19 20 21 22  |
#   |  21 22 23 24 25  |  25 26 27 28 29  |  23 24 25 26 27  |
#   |  26 27 28 29 30  |  30              |  28 29 30 31     |
#   |  31              |                  |                  |
#   |--------------------------------------------------------|
#
#-----------------------------------------------------------------------------|
#-----------------------------------------------------------------------------|
#
# * Use 4 level nested loop to display 3 days of the week, for the first two full weeks of the month, for 3 months, for 2 years.
#   perl ezdates.pl #reference-date#; date 2007-12-25; #for-each-year#; loop 2; #for-each-month#; loop 3; 1st full week of month; calendar; print; #for-each-week#; loop 2; start-of-week; #for-each-day#; loop 3; add +1; print; #end-day-loop#; end; end-of-week; add +1; #end-week-loop#; end; end-of-month; add +1; #end-month-loop#; end; #loop-month-till-next-year#; loop 9; end-of-month; add +1; #end-loop-month-till-next-year#; end; #end-year-loop#; end; noprint;
#
# * Results:
#         2007-12
#   Su Mo Tu We Th Fr Sa
#                     01
#   02 03 04 05 06 07 08
#   09 10 11 12 13 14 15
#   16 17 18 19 20 21 22
#   23 24 25 26 27 28 29
#   30 31
#   2007-12-03
#   2007-12-04
#   2007-12-05
#   2007-12-10
#   2007-12-11
#   2007-12-12
#         2008-01
#   Su Mo Tu We Th Fr Sa
#         01 02 03 04 05
#   06 07 08 09 10 11 12
#   13 14 15 16 17 18 19
#   20 21 22 23 24 25 26
#   27 28 29 30 31
#   2008-01-07
#   2008-01-08
#   2008-01-09
#   2008-01-14
#   2008-01-15
#   2008-01-16
#         2008-02
#   Su Mo Tu We Th Fr Sa
#                  01 02
#   03 04 05 06 07 08 09
#   10 11 12 13 14 15 16
#   17 18 19 20 21 22 23
#   24 25 26 27 28 29
#   2008-02-04
#   2008-02-05
#   2008-02-06
#   2008-02-11
#   2008-02-12
#   2008-02-13
#         2008-12
#   Su Mo Tu We Th Fr Sa
#      01 02 03 04 05 06
#   07 08 09 10 11 12 13
#   14 15 16 17 18 19 20
#   21 22 23 24 25 26 27
#   28 29 30 31
#   2008-12-08
#   2008-12-09
#   2008-12-10
#   2008-12-15
#   2008-12-16
#   2008-12-17
#         2009-01
#   Su Mo Tu We Th Fr Sa
#               01 02 03
#   04 05 06 07 08 09 10
#   11 12 13 14 15 16 17
#   18 19 20 21 22 23 24
#   25 26 27 28 29 30 31
#   2009-01-05
#   2009-01-06
#   2009-01-07
#   2009-01-12
#   2009-01-13
#   2009-01-14
#         2009-02
#   Su Mo Tu We Th Fr Sa
#   01 02 03 04 05 06 07
#   08 09 10 11 12 13 14
#   15 16 17 18 19 20 21
#   22 23 24 25 26 27 28
#   2009-02-02
#   2009-02-03
#   2009-02-04
#   2009-02-09
#   2009-02-10
#   2009-02-11
#
#-----------------------------------------------------------------------------|

