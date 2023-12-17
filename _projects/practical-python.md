---
layout: page
title: Practical Python
description: Programming fundamentals
img: # assets/img/beer.jpg # TODO: Pick an image
importance: 8
category: personal
---
<style>
.image-container {
    height: 500px; /* Adjust as needed */
    border-radius: .25rem; /* To match rounded style */
    background-position: center center; /* Adjust as needed */
    background-size: cover;
    box-shadow: 0 2px 2px 0 rgba(0, 0, 0, 0.14), 0 3px 1px -2px rgba(0, 0, 0, 0.12), 0 1px 5px 0 rgba(0, 0, 0, 0.2); /* For z-depth-1 effect */
}
</style>

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        <p>
        I completed this course because I wanted to do a structured introduction to Python. As part of my schoolwork, we were using Python for machine learning, but I kept finding myself wanting to do more with it and learning one small bit at a time. I learned Java as an undergrad, and have done a lot of work in R, but I needed to formally brush up on object-oriented programming. Python's quick and scripty nature also appealed to me so I chose to learn more about the language.
        </p>
        <p>
        <a href="https://dabeaz.com">David Beazley</a>'s python courses stood out to me when looking for resources, and came highly recommended. This course, <a href="https://dabeaz-course.github.io/practical-python">Practical Python</a>, is meant to be taught in person over a week and consists of about 130 exercises. The author has made the course available <a href="https://github.com/dabeaz-course/practical-python">here</a> under the Creative Commons license. It focuses on script writing, data manipulation, and program organization. I liked that course focused on basic python functionality and didn't require any external libraries. Mastering the basics is the best way to proficiency in anything.
        </p>
        <p>
        Some of the exercises I completed for the course are shown below. Most were iterative and the concepts built on each other, so the versions shown below represent the final iteration of the code. To get the most out of this course, I completed these exercises without Copilot or help from LLM's.
        </p>
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        <div class="image-container" style="background-image: url('/assets/img/pr-python.png');"></div>
    </div>

</div>
<div class="caption">
    All of my work from this course can be found in the <a href="https://github.com/NickSager/practical-python/tree/master/Work/porty-app/porty">practical-python</a> repository on my GitHub.
</div>

### Reading Data

In this exercise, I created a script that would parse a CSV file and return a list of records. The script is flexible and can take a file object or a list of strings as input. It can also take a list of column names to select, a list of types to convert the data to, and a delimiter. It also has the ability to skip rows with no data and to silence errors. This exercise was interesting because the options necessitate some complex control logic, and it required logging and error handling. Something like `pandas` would be a better choice for this task, but it was fun to learn how to do it using just basic python.

```python
# fileparse.py
#
# Exercise 8.2
import csv
import logging
log = logging.getLogger(__name__)


def parse_csv(lines, select=None, types=None, has_headers=True,
              delimiter=',', silence_errors=False):
    '''
    Parses lines from a CSV file into a list of records
    Takes only lines from a file - allows flexibility of opening differently
    Typical usage:
        with open('file.csv') as f:
            x = parse_csv(f)
    Note: types needs to match select or row in length
        Implement error handling here later
    '''
    if select and not has_headers:
        raise RuntimeError('Select argument requires column headers')

    rows = csv.reader(lines, delimiter=delimiter)

    # Read the file headers
    headers = next(rows) if has_headers else []

    if select:
        indices = [headers.index(colname) for colname in select]
        headers = select

    records = []
    for rowno, row in enumerate(rows, 1):
        if not row:  # Skip rows with no data "Truthiness"
            continue

        if select:
            row = [row[index] for index in indices]

        if types:
            try:
                row = [func(val) for func, val in zip(types, row)]
            except ValueError as e:
                if not silence_errors:
                    log.warning("Row %d: Couldn't convert %s", rowno, row)
                    log.debug("Row %d: Reason %s", rowno, e)
                    # print(f"Row {rowno}: Couldn't convert {row}")
                    # print(f"Row {rowno}: Reason {e}")
                    continue

        if has_headers:
            record = dict(zip(headers, row))
        else:
            record = tuple(row)
        records.append(record)

    return records
```


### Printing a Report

This exercise was about creating a report from a list of portfolio holdings. The script takes a portfolio and a list of prices and creates a report. It can output the results in a variety of formats, including text, csv, and html. It makes use of a number of other modules I built throughout the course, and got updated as the course introduced new topics. See `tableformat.py` for how the different formats are handled.

Here is a sample of the output in text format:
```
      Name     Shares      Price     Change 
---------- ---------- ---------- ---------- 
        AA        100       9.22     -22.98 
       IBM         50     106.28      15.18 
       CAT        150      35.46     -47.98 
      MSFT        200      20.89     -30.34 
        GE         95      13.48     -26.89 
      MSFT         50      20.89     -44.21 
       IBM        100     106.28      35.84 
---------- ---------- ---------- ---------- 
                           Value $ 28,686.10
                      Net Change $-15,985.05
```

```python
#!/usr/bin/env python3
# report.py
#
# Exercise 7.4
from . import fileparse
from .stock import Stock
from .portfolio import Portfolio
from . import tableformat


def read_portfolio(filename, **opts):
    '''
    Read a stock portfolio file into a list of dictionaries with items
    name, shares, and price.
    '''
    with open(filename) as file:
        portfolio = Portfolio.from_csv(file)
    return portfolio


def read_prices(filename, select=None, types=[str, float], has_headers=False,
                delimiter=',', silence_errors=False):
    '''
    Read a CSV file of price data into a dict mapping names to prices.
    '''
    with open(filename) as file:
        prices = fileparse.parse_csv(file, select=select, types=types,
                                     has_headers=has_headers, delimiter=delimiter,
                                     silence_errors=silence_errors)
    prices = dict(prices)
    return (prices)


def make_report_data(portfolio, prices):
    '''
    Create a report from a list of portfolio holdings.
    '''
    report = []
    for holding in portfolio:
        current_price = prices[holding.name]
        change = current_price - holding.price
        summary = (holding.name, holding.shares, current_price, change)
        report.append(summary)
    return (report)


def print_report(report, formatter):
    '''
    Print a nicely formated table from a list of (name, shares, price, change)
    takes a report object, portfolio, and prices.
    '''

    current_value = 0.0  # Can do here, or loop again in footer
    net_change = 0.0

    # Header
    formatter.headings(['Name', 'Shares', 'Price', 'Change'])

    # Table Body
    for name, shares, price, change in report:
        rowdata = [name, str(shares), f'{price:0.2f}', f'{change:0.2f}']
        formatter.row(rowdata)
        # Iterate here or loop again in footer
        current_value += shares*price
        net_change += shares*change

    # Separator and Footer
    footer = {'Value': current_value, 'Net Change': net_change}
    formatter.footer(footer)


def portfolio_report(portfoliofile, pricefile, fmt='txt'):
    '''
    Make a stock report given portfolio and price data files.
    fmt = 'txt', 'csv', 'html'
    '''
    # Read data files
    portfolio = read_portfolio(portfoliofile)
    prices = read_prices(pricefile)

    # Create the report Data
    report = make_report_data(portfolio, prices)

    # Print the report
    formatter = tableformat.create_formatter(fmt)
    print_report(report, formatter)


def main(argv):
    # Default values for arguments
    default_portfoliofile = './portfolio.csv'
    default_pricefile = './prices.csv'
    default_fmt = 'txt'

    # Use the default arguments if not provided
    portfoliofile = argv[1] if len(argv) > 1 else default_portfoliofile
    pricefile = argv[2] if len(argv) > 2 else default_pricefile
    fmt = argv[3] if len(argv) > 3 else default_fmt

    # Now call the ticker function with the arguments
    portfolio_report(portfoliofile, pricefile, fmt)

    # Old way
    # if len(argv) == 4:
    #     portfolio_report(argv[1], argv[2], argv[3])
    # elif len(argv) == 3:
    #     portfolio_report(argv[1], argv[2])
    # else:
    #     raise SystemExit('Usage: %s portfile pricefile format' % argv[0])


if __name__ == "__main__":
    import sys
    # This file sets up basic configuration of the logging module.
    # Change settings here to adjust logging output as needed.
    import logging
    logging.basicConfig(
        # filename = 'app.log',            # Name of the log file (omit to use stderr)
        # filemode = 'w',                  # File mode (use 'a' to append)
        level    = logging.WARNING,      # Logging level (DEBUG, INFO, WARNING, ERROR, or CRITICAL)
    )
    main(sys.argv)
```
### Miscellaneous

The course also included a number of other exercises that I found interesting. For example, we packaged all of the stock portfolio related code into a package and learned how to use `setup.py` to install or distribute it. The section on generators was really interesting and I plan to revisit that topic. I learned about decorators and in the example below, used them to create typed properties that could be used in the other modules. 

Overall, I found this course to be a great introduction to Python. I've been surprised at how useful these basic skills have been in other projects, and I'm glad I spent the time to learn them. 

```python
#!/usr/bin/env python3
# typedproperty.py
#
# Exercise 7.7

def typedproperty(name, expected_type):
    private_name = '_' + name

    @property
    def prop(self):
        return getattr(self, private_name)

    @prop.setter
    def prop(self, value):
        if not isinstance(value, expected_type):
            raise TypeError(f'Expected {expected_type}')
        setattr(self, private_name, value)

    return prop

# With Lambda Functions:
# String = lambda name: typedproperty(name, str)
# Integer = lambda name: typedproperty(name, int)
# Float = lambda name: typedproperty(name, float)

# With short definitions (lambda discouraged by linter)
def String(name): return typedproperty(name, str)
def Integer(name): return typedproperty(name, int)
def Float(name): return typedproperty(name, float)

```
