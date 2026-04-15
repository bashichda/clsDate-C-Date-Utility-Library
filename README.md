# 📅 clsDate — C++ Date Utility Library

![C++](https://img.shields.io/badge/Language-C%2B%2B-blue?style=flat-square&logo=cplusplus)
![Platform](https://img.shields.io/badge/Platform-Windows%20%2F%20Cross--platform-lightgrey?style=flat-square)
![Type](https://img.shields.io/badge/Type-Header--only-orange?style=flat-square)
![Dependency](https://img.shields.io/badge/Dependency-clsString-purple?style=flat-square)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)

A comprehensive, header-only C++ date utility class that wraps common date operations — arithmetic, comparison, formatting, calendar printing, and vacation/business day calculations — behind a clean, intuitive API.

> 🔗 **Dependency:** Requires `clsString.h` for the string-based constructor (`clsString::Split`).

---

## ✨ Features

- 🏗️ **Multiple constructors** — default (today), string `"D/M/Y"`, day/month/year components, or day-of-year + year
- ➕ **Date arithmetic** — add or subtract days, weeks, months, years, decades, centuries, and millennia
- 📊 **Comparison** — before, equal, after — static and instance versions with a typed `enDateCompare` enum
- 📅 **Calendar printing** — formatted month or full year calendar printed to stdout
- 💼 **Business days** — count business days between two dates; compute vacation return dates
- 🔍 **Validation** — `IsValid`, `IsLeapYear`, `IsWeekEnd`, `IsBusinessDay`, `IsLastDayInMonth`
- 🗓️ **Day-of-week** — Gregorian weekday order, short day name, short month name
- ⏳ **Difference in days** — signed difference between two dates; age in days from date of birth

---

## 📸 Quick Preview

```cpp
clsDate d1(15, 6, 2024);
d1.Print();                    // 15/6/2024
d1.AddOneDay();                // → 16/6/2024
d1.IncreaseDateByXMonths(3);   // → 16/9/2024

clsDate::PrintMonthCalendar(6, 2024);
// _______________Jun_______________
//  Sun  Mon  Tue  Wed  Thu  Fri  Sat
//                                 1
//    2    3    4    5    6    7    8
//  ...

int diff = clsDate::GetDifferenceInDays(
    clsDate(1, 1, 2024), clsDate(1, 1, 2025));
// diff = 365
```

---

## 🗂️ Project Structure

```
YourProject/
├── clsDate.h       ← this library (header-only)
└── clsString.h     ← required: provides clsString::Split()
```

---

## 🚀 Getting Started

### Prerequisites

- A C++ compiler with **C++11 or later** (MSVC, MinGW, or GCC)
- `clsString.h` must be in your include path

### Installation

Header-only — just copy the files into your project and include:

```cpp
#include "clsDate.h"
```

---

## 🧱 Constructors

| Constructor | Description |
|---|---|
| `clsDate()` | Initialises to today's system date |
| `clsDate("15/6/2024")` | Parses a `"D/M/Y"` string |
| `clsDate(15, 6, 2024)` | Explicit day, month, year |
| `clsDate(167, 2024)` | Day-order-in-year + year (e.g. the 167th day of 2024) |

---

## 📚 API Reference

### Properties (get / put)

| Property | Type | Notes |
|---|---|---|
| `Day` | `short` | 1–31 |
| `Month` | `short` | 1–12 |
| `Year` | `short` | Any year |

### Formatting & Output

| Method | Description |
|---|---|
| `Print()` | Prints `"D/M/Y"` to stdout |
| `DateToString()` | Returns `"D/M/Y"` as `std::string` |
| `DayShortName()` | Returns e.g. `"Mon"` |
| `MonthShortName()` | Returns e.g. `"Jun"` |
| `PrintMonthCalendar()` | Prints a grid calendar for the date's month |
| `PrintYearCalendar()` | Prints all 12 months of the date's year |

### Arithmetic (instance methods)

| Method | Description |
|---|---|
| `AddOneDay()` / `AddDays(n)` | Advances by 1 or n days |
| `IncreaseDateByOneWeek()` / `...XWeeks(n)` | Adds 7 or 7×n days |
| `IncreaseDateByOneMonth()` / `...XMonths(n)` | Adds months, clamps day to month end |
| `IncreaseDateByOneYear()` / `...XYears(n)` | Increments year |
| `IncreaseDateByOneDecade()` / `...XDecades(n)` | Adds 10 or 10×n years |
| `IncreaseDateByOneCentury()` | Adds 100 years |
| `DecreaseDateBy...()` | Symmetric decrease variants for all of the above |

### Comparison & Validation

| Method | Returns | Description |
|---|---|---|
| `IsValid()` | `bool` | Full date validity check |
| `isLeapYear()` | `bool` | Gregorian leap year rule |
| `IsWeekEnd()` | `bool` | True for Fri or Sat |
| `IsBusinessDay()` | `bool` | Inverse of `IsWeekEnd` |
| `IsLastDayInMonth()` | `bool` | True if day = last day of month |
| `IsDateBeforeDate2(d2)` | `bool` | Chronological less-than |
| `IsDateEqualDate2(d2)` | `bool` | Exact match |
| `IsDateAfterDate2(d2)` | `bool` | Chronological greater-than |
| `CompareDates(d2)` | `enDateCompare` | Returns `Before / Equal / After` |

### Calculations

| Method | Description |
|---|---|
| `GetDifferenceInDays(d2, includeEnd)` | Signed day count between two dates |
| `DaysFromTheBeginingOfTheYear()` | Day ordinal within the year (1-based) |
| `DaysUntilTheEndOfWeek()` | Days remaining until Saturday |
| `DaysUntilTheEndOfMonth()` | Days remaining in the current month |
| `DaysUntilTheEndOfYear()` | Days remaining in the current year |
| `CalculateBusinessDays(from, to)` *(static)* | Count of Mon–Thu days in range |
| `CalculateVacationReturnDate(from, n)` *(static)* | Return date after n vacation days (skips weekends) |
| `CalculateMyAgeInDays(dob)` *(static)* | Age in days from date of birth to today |

---

## 🧠 Design Notes

### Weekday Algorithm

Uses a Gregorian-based formula (similar to Zeller's congruence):

```
(Day + y + y/4 - y/100 + y/400 + 31m/12) % 7
```

Where `0 = Sunday`, `1 = Monday`, …, `6 = Saturday`.

### Weekend Definition

Weekends are defined as **Friday (5) and Saturday (6)**, and business days are everything else. Adjust `IsWeekEnd()` if your locale uses a different weekend.

### Month-End Clamping

When adding months (e.g. `31/1/2024 + 1 month`), the day is clamped to the last valid day of the target month — so the result is `29/2/2024` (leap year), not an invalid date.

---

## 🛠️ Technologies Used

- **Language:** C++
- **Libraries:** `<iostream>`, `<string>`, `<ctime>`, `clsString`
- **Concepts:** Header-only class, Gregorian calendar arithmetic, MSVC properties, static/instance method duality, date validation

---

## 🔮 Possible Improvements

- [ ] Remove MSVC-specific `__declspec(property)` for full cross-platform support
- [ ] Add time-zone awareness
- [ ] Implement operator overloads (`+`, `-`, `<<`) for idiomatic C++ usage
- [ ] Replace `printf`-based calendar with `std::cout`
- [ ] Add a `clsDate + int` operator for day-based arithmetic
- [ ] Unit test suite (Google Test / Catch2)

---

## 👨‍💻 Author

> Built with ❤️ as part of a C++ learning journey.

Feel free to fork, star ⭐, or contribute!

---

## 📄 License

This project is licensed under the **MIT License** — free to use and modify.
