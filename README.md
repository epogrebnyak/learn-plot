Visualisation 
=============

Goals: 
------

- [ ] sort out code written at different time for plotting
- [ ] adopt fast declarative plotting with altair 
- [ ] list videos about plotting

Videos:
-------

- altair
- Pydata

Repos:
------

Plotting repositories are below, need to extract useful parts of it and delete the rest:

- https://github.com/epogrebnyak/viz_demo
- https://github.com/epogrebnyak/plotting
- https://github.com/epogrebnyak/Business-Conditions-Digest-2017
- https://github.com/epogrebnyak/data-lab
- https://github.com/mini-kep/user-charts
- https://github.com/mini-kep/parser-rosstat-kep/tree/dev-hidden/hidden/code_extras


Plotting at SO
---------------

- [3d plot of tough regression](https://stackoverflow.com/questions/51874840/polynomial-regression-wont-work/51880793#51880793)
- [what's on histogram?](https://stackoverflow.com/questions/50669200/distribution-plot-in-python/50669428#50669428)


Altair
------

- see video of overview


Addition from gist
------------------

```python
# -*- coding: utf-8 -*-
"""
Created on Thu Jul 13 13:56:47 2017

@author: PogrebnyakEV
"""

from ad2 import get_dataframes

import plotly 
plotly.tools.set_credentials_file(username='e.pogrebnyak', 
                                  api_key='e1PQBOnueaEKePJqeEqw')

import plotly.plotly as py
import plotly.graph_objs as go



dfq, dfq, dfm = get_dataframes()

_label = 'RETAIL_SALES_yoy'
_desc = "Оборот розничной торговли, изменение за 12 мес."
_freq_label = 'm_RETAIL_SALES_yoy'

_df = dfm[_label]
data = [go.Scatter(
          x=_df.index,
          # FIXME: not proper rounding for exchange rates
          y=[round(x, 1) for x in list(_df)]
          )]


layout = dict(
    title="", # 'Time Series with Rangeslider',
    xaxis=dict(
        rangeselector=dict(
            buttons=list([
                dict(count=12,
                     label='12m',
                     step='month',
                     stepmode='backward'),
                dict(count=12*3,
                     label='3y',
                     step='month',
                     stepmode='backward'),
                dict(step='all')
            ])
        ),
        rangeslider=dict(),
        type='date'
    )
)

fig = dict(data=data, layout=layout)
py.iplot(fig, filename = "Monthly retail sales")

def get_timeseries(varname, freq, mod):
    """
---------------------------|--------|----------|---------------|
                           | Volume | % change |    % change   |
                           |        |   (t-1)  | (m-12 or q-4) |
---------------------------|--------|----------|---------------|
 Nominal at current prices | 'nom'  |          |               |
---------------------------|--------|----------|---------------|
 Real at base prices       | 'base' |  'rog'   |     'yoy'     |
---------------------------|--------|----------|---------------|
"""
    pass   


```

slider.py
---------

```
from bokeh.io import output_file, show
from bokeh.layouts import widgetbox
from bokeh.models.widgets.inputs import DateRangeSlider

output_file("layout_widgets.html")

start = '2010-01-01'
end = '2015-01-01'

beginning = DateRangeSlider(
    title="Period:", name="period", value=(start,end),
    bounds=(start,end), range=(dict(days=1), None))

show(widgetbox(beginning))
```
