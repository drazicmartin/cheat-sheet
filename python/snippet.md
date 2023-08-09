# Python Snippet code

## Open CV
### Show image with quick resize
```python
img_tmp = img; scale_tmp=50; cv2.imshow("window_name", cv2.resize(img_tmp, (int(img_tmp.shape[1]*(scale_tmp/100)), int(img_tmp.shape[0]*(scale_tmp/100))), interpolation=cv2.INTER_CUBIC));cv2.waitKey(0);cv2.destroyAllWindows()
```
### read video
```python
TODO
```

## Numpy

### Numpy
save
```python
# Single
np.save("test.npy", a)
  
# Multy
with open('test.npy', 'wb') as f:
  np.save(f, a)
  np.save(f, b)
```
load
```python
# Single
  
a = np.load("test.npy")

# Multy
with open('test.npy', 'rb') as f:
  a = np.load(f)
  b = np.load(f)
```
Flatten / Ravel / Unfold Array
```python
ndarray.flatten(order='C')

np.ravel(a, order='C')
# is equivalent to :
ndarray.reshape(-1, order=order)
```

## Multi processing / Muli threding
  
```python
from tqdm.contrib.concurrent import process_map
r = process_map(process_fct, data_all_splited_pid)
```
## Pandas
### Concat Dataframe
```python
<DataFrame> = pd.concat((<DataFrame>, <Series>.to_frame().T), ignore_index=True)
```
## hash
  
```python
 hashlib.md5(bytes(row['video_path'], 'utf-8')).hexdigest()
```

## 3D vis plt with annotations
```python
fig = plt.figure()
ax = fig.add_subplot(projection='3d')
for txt, (x,y,z) in zip(joint_names, det['pred_xyz_jts']):
    ax.scatter(x,y,z, marker="^", alpha=1)
    ax.text(x,y,z, txt)
plt.show()
```

## Fichier PKL
### write
```python
with open('filename.pkl', 'wb') as f:
# dump information to that file
pickle.dump(data, f)
```
### read
```python
with open('filename.pkl', 'rb') as f:
# dump information to that file
data = pickle.load(f)
```
## Pathlib
	
```python
p = Path(r"C:\data\test_samples\alley.mp4")

p.name
# -> 'alley.mp4'

p.parent
# -> 'C:\data\test_samples'

p.is_file()
# -> bool
p.is_dir()
# -> bool
p.mkdir()
```

## JSON
### Write
```python
with open("sample.json", "w") as outfile:
json.dump(dictionary, outfile)
```
### Read
```python
with open('sample.json', 'r') as f:
data = json.loads(f.read())
```

## Count execution time
```python
# include time elapsed during sleep and is system-wide
time.perf_counter()

# It does not include time elapsed during sleep. It is process-wide by definition
time.process_time()

time.time()

from timeit import default_timer as timer
start = timer()
# ...
end = timer()
```

## Pytorch / Tensor
```python
x is torch.Tensor

x.shape
# (50, 2)

x.unsqueeze(0)
# (1, 50, 2)
```

## YAML files
### Read
```python
import yaml
from yaml.loader import SafeLoader

# Open the file and load the file
with open('Userdetails.yaml') as f:
    data = yaml.load(f, Loader=SafeLoader)
    print(data)
```
- BaseLoader: Loads all the basic YAML scalars as Strings
- SafeLoader: Loads subset of the YAML safely, mainly used if the input is from an untrusted source.
- FullLoader: Loads the full YAML but avoids arbitrary code execution. Still poses a potential risk when used for the untrusted input.
- UnsafeLoader: Original loader for untrusted inputs and generally used for backward compatibility.
- Note: It is always safe t
- Write
```python
import yaml

# dict object
members = [{'name': 'Zoey', 'occupation': 'Doctor'},
           {'name': 'Zaara', 'occupation': 'Dentist'}]

# Convert Python dictionary into a YAML document
print(yaml.dump(members))
```

## Glob multiple extension

## Regex

```python
import re

pattern = r"pose_(\d+).pkl$"
text = "some_text"

match = re.match(pattern, text)
# return -> None || MatchObject
found = re.findall(pattern, text)
# return -> list
	  ```
