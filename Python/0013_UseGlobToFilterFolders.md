## Usage
- Determine a path as a file ` os.path.isfile() `
- Determine a path as a folder ` os.path.isdir() `
- Search under a path ` glob.glob() `
- Get file name from a given path ` os.path.basename(sub_file) `

## Code snap to filter under a path
```python
    import glob
    import os
    
    target_folder = "xxxxx"
    arr_dir = []
    if os.path.exists(target_folder):
      for sub_file in glob.glob(target_folder + "/*"):
        if os.path.isdir(sub_file):
          arr_dir.append(sub_file)
```

## References
- http://python.jobbole.com/81552/

## Back to [index](./index.md)
