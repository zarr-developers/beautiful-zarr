# [marynaggita](https://github.com/marynaggita/)'s Zarr Collection

## Tweet Mentions

- https://twitter.com/latlong_blog/status/1543726888908009472
- https://twitter.com/_jhamman/status/1579882167303168004
- https://twitter.com/ErikvanSebille/status/1578030942144188417



## Zarr Data Visualisation
![image1](/_data/marynaggita/screenshots/data2.PNG)


Use Case: Study Amazon Estuaries with Data from the EOSDIS Cloud(https://github.com/podaac/tutorials/blob/master/notebooks/SWOT-EA-2021/Estuary_explore_inCloud_zarr.ipynb) 


## Code Snippets


NASA Earthdata Login Setup

An Earthdata Login account is required to access data, as well as discover restricted data, from the NASA Earthdata system. Please visit https://urs.earthdata.nasa.gov to register and manage your Earthdata Login account. This account is free to create and only takes a moment to set up.

The setup_earthdata_login_auth function will allow Python scripts to log into any Earthdata Login application programmatically. To avoid being prompted for credentials every time you run and also allow clients such as curl to log in, you can add the following to a .netrc (_netrc on Windows) file in your home directory:

machine urs.earthdata.nasa.gov
    login <your username>
    password <your password>

Make sure that this file is only readable by the current user or you will receive an error stating "netrc access too permissive."

$ chmod 0600 ~/.netrc

You will be prompted for your username and password if you dont have a netrc file. Note: these imports are all in the Python 3.6+ standard library.


Code 👇🏻

```
import time
import requests
import numpy as np
import pandas as pd
import xarray as xr
import matplotlib.pyplot as plt
import matplotlib.animation as animation
import cartopy.crs as ccrs
import cartopy
import zarr
import s3fs
from IPython.display import HTML
from json import dumps, loads

cmr = "cmr.earthdata.nasa.gov"
urs = "urs.earthdata.nasa.gov"
harmony = "harmony.earthdata.nasa.gov"

from platform import system
from netrc import netrc
from getpass import getpass
from urllib import request
from http.cookiejar import CookieJar
from os.path import join, expanduser

TOKEN_DATA = ("<token>"
              "<username>%s</username>"
              "<password>%s</password>"
              "<client_id>PODAAC CMR Client</client_id>"
              "<user_ip_address>%s</user_ip_address>"
              "</token>")


def setup_earthdata_login_auth(urs: str='urs.earthdata.nasa.gov', cmr: str='cmr.earthdata.nasa.gov'):

    # GET URS LOGIN INFO FROM NETRC OR USER PROMPTS:
    netrc_name = "_netrc" if system()=="Windows" else ".netrc"
    try:
        username, _, password = netrc(file=join(expanduser('~'), netrc_name)).authenticators(urs)
        print("# Your URS credentials were securely retrieved from your .netrc file.")
    except (FileNotFoundError, TypeError):
        print('# Please provide your Earthdata Login credentials for access.')
        print('# Your info will only be passed to %s and will not be exposed in Jupyter.' % (urs))
        username = input('Username: ')
        password = getpass('Password: ')

    # SET UP URS AUTHENTICATION FOR HTTP DOWNLOADS:
    manager = request.HTTPPasswordMgrWithDefaultRealm()
    manager.add_password(None, urs, username, password)
    auth = request.HTTPBasicAuthHandler(manager)
    jar = CookieJar()
    processor = request.HTTPCookieProcessor(jar)
    opener = request.build_opener(auth, processor)
    request.install_opener(opener)

    # GET TOKEN TO ACCESS RESTRICTED CMR METADATA:
    ip = requests.get("https://ipinfo.io/ip").text.strip()
    r = requests.post(
        url="https://%s/legacy-services/rest/tokens" % cmr,
        data=TOKEN_DATA % (str(username), str(password), ip),
        headers={'Content-Type': 'application/xml', 'Accept': 'application/json'}
    )
    return r.json()['token']['id']


    
	_token = setup_earthdata_login_auth(urs=urs, cmr=cmr)
	r = requests.get(url=f"https://{cmr}/search/collections.umm_json", 
                 params={'provider': "POCLOUD", 
                         'ShortName': grace_ShortName, 
                         'token': _token})

grace_coll = r.json()

grace_coll_meta = grace_coll['items'][0]['meta']
r = requests.get(url=f"https://{cmr}/search/granules.umm_json", 
                 params={'provider': "POCLOUD", 
                         'ShortName': grace_ShortName, 
                         'token': _token})

grace_gran = r.json()
grace_gran['hits']
grace_gran['items'][0]['umm']['RelatedUrls']
grace_url = grace_gran['items'][0]['umm']['RelatedUrls'][1]['URL']

async_jobId = None 
collection_concept_id = grace_coll_meta['concept-id']
async_url = f'https://{harmony}/{collection_concept_id}/ogc-api-coverages/1.0.0/collections/all/coverage/rangeset?format=application/x-zarr'
if async_jobId is None:
    print('Request URL: ', async_url)
    async_response = request.urlopen(async_url)
    async_results = async_response.read()
    async_json = loads(async_results)
    print(dumps(async_json, indent=2))
    job_url = f'https://{harmony}/jobs/{async_jobId}'
    while True:
    loop_response = request.urlopen(job_url)
    loop_results = loop_response.read()
    job_json = loads(loop_results)
    if job_json['status'] != 'running':
        break
    print(f"# Job status is running. Progress is {job_json['progress']}. Trying again.")
    time.sleep(5)

links = []
if job_json['status'] == 'successful' and job_json['progress'] == 100:
    print("# Job progress is 100%. Links to job outputs are displayed below:")
    links = [link['href'] for link in job_json['links']]
    display(links)
    zarr_url = links[1]
    with request.urlopen(f"https://{harmony}/cloud-access") as f:
    creds = loads(f.read())

creds['Expiration']
zarr_fs = s3fs.S3FileSystem(
    key=creds['AccessKeyId'],
    secret=creds['SecretAccessKey'],
    token=creds['SessionToken'],
    client_kwargs={'region_name':'us-west-2'},
)
zarr_store = zarr_fs.get_mapper(root=zarr_url, check=False)
zarr_dataset = zarr.open(zarr_store)

print(zarr_dataset.tree())

print(zarr_dataset.lwe_thickness.info)
ds_GRACE = xr.open_zarr(zarr_store)
print(ds_GRACE)

subset_GRACE = ds_GRACE.sel(lat=slice(-18, 10), lon=slice(275, 330))
print(subset_GRACE.lat.min().data, 
      subset_GRACE.lat.max().data,
      subset_GRACE.lon.min().data,
      subset_GRACE.lon.max().data)



lwe = subset_GRACE.lwe_thickness


def setup_map(ax, pmap, ds_subset, x, y, var, t, cmap, levels, extent):
    title = str(pd.to_datetime(ds_subset.time[t].values))
    pmap.set_title(title, fontsize=14)
    pmap.coastlines()
    pmap.set_extent(extent)
    pmap.add_feature(cartopy.feature.RIVERS)
    variable_desired = var[t,:,:]
    cont = pmap.contourf(x, y, variable_desired, cmap=cmap, levels=levels, zorder=1)
    return cont

def animate_ts(framenumber, ax, pmap, ds_subset, x, y, var, t, cmap, levels, extent):
    cont = setup_map(ax, pmap, ds_subset, x, y, var, t + framenumber, cmap, levels, extent) 
    return cont

# Initialize a matplotlib plot object and add subplot:
fig = plt.figure(figsize=[13,9]) 
ax = fig.add_subplot(1, 1, 1)

# Configure axes to display projected data using PlateCarree crs:
pmap = plt.axes(projection=ccrs.PlateCarree())

# Get arrays of x and y to label the plot axes:
x,y = np.meshgrid(subset_GRACE.lon, subset_GRACE.lat)                        

# Set a few constants for plotting the GRACE-FO time series:
time_start  = 168
cmap_name   = "bwr_r"
cmap_levels = np.linspace(-100., 100., 14)
map_extent  = [-85, -30, -16, 11]

# Plot the first timestep: 
cont = setup_map(ax, pmap, subset_GRACE, x, y, lwe, time_start, cmap_name, cmap_levels, map_extent)

fig.colorbar(cont, ticks=cmap_levels, orientation='horizontal', label='Land Water Equivalent Thickness (cm)')
```

![image2](/_data/marynaggita/screenshots/data.PNG)