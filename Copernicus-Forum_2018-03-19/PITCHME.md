## Application Structure

---

### Application: Compute Climatology

```python
from cdstoolbox import app
from cdstoolbox.tools import cdstools as ct
from cdstoolbox.tools import cdsplot as cp


@app.application(title='Compute Climatology')
@app.input.dropdown('frequency', default='month', values=['dayofyear', 'weekofyear', 'month'], link=True)
@app.input.dropdown('start', when='dayofyear', default=270, values=[d + 1 for d in range(366)])
@app.input.dropdown('start', when='weekofyear', default=46, values=[w + 1 for w in range(53)])
@app.input.dropdown('start', when='month', default=10, values=app.month_range(1, 12))
@app.output.livefigure()
def compute_climatology(
        frequency,
        start
):
    """
    Compute climatology mean and standard deviation for a specific location.
    Plot results on a line plot.
    """
    data = ct.catalogue.retrieve('ERA-Interim', 'tas', 'day')
    
    data_location = ct.lonlat.extract_point(data, lon=-1, lat=51.5)
    clima_mean = ct.season.climatology_mean(data_location, frequency=frequency)
    clima_std = ct.season.climatology_std(data_location, frequency=frequency)
    
    fig = cp.livefigure(
        layout={'title': 'Climatology mean and standard deviation - Starting from %s n. %s' % (frequency, int(start))}
    )
    cp.plot_climatology(clima_mean, fig=fig, start_from=int(start), error_y=clima_std)
    
    return fig
```

---

### Import
```python
from cdstoolbox import app
from cdstoolbox.tools import cdstools as ct
from cdstoolbox.tools import cdsplot as cp


@app.application(title='Compute Climatology')
@app.input.dropdown('frequency', default='month', values=['dayofyear', 'weekofyear', 'month'], link=True)
@app.input.dropdown('start', when='dayofyear', default=270, values=[d + 1 for d in range(366)])
@app.input.dropdown('start', when='weekofyear', default=46, values=[w + 1 for w in range(53)])
@app.input.dropdown('start', when='month', default=10, values=app.month_range(1, 12))
@app.output.livefigure()
def compute_climatology(
        frequency,
        start
):
    """
    Compute climatology mean and standard deviation for a specific location.
    Plot results on a line plot.
    """
    data = ct.catalogue.retrieve('ERA-Interim', 'tas', 'day')
    
    data_location = ct.lonlat.extract_point(data, lon=-1, lat=51.5)
    clima_mean = ct.season.climatology_mean(data_location, frequency=frequency)
    clima_std = ct.season.climatology_std(data_location, frequency=frequency)
    
    fig = cp.livefigure(
        layout={'title': 'Climatology mean and standard deviation - Starting from %s n. %s' % (frequency, int(start))}
    )
    cp.plot_climatology(clima_mean, fig=fig, start_from=int(start), error_y=clima_std)
    
    return fig
```
@[1-3]
@[5-15]
@[20]
@[22-24]
@[26-31]

---
