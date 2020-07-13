# yaml-to-table
Convert YAML file to text/html table for documentation 

# Introduction 
  Time to time I needed to document YAML files for end-users. And best way so far we have seen is to build
  table and put each field in table row and document it. 
  I need a quick script that will take YAML file and generated (html) table;
   only thing I needed to do was to input some help text that explains field
   
   Above need gave way to this python script; it will do just that - given input (yaml) file it will   
   it iterates over each yaml section and builds table(s) out of it 
   
   As of now it creates multiple tables based on top level YAML sections  
   
   This mainly uses two python libraries '*pyaml*' and '*prettyTable*' 
   
   [PYAML](https://pyyaml.org/wiki/PyYAML): For reading the yaml file    
   [PrettyTable](https://pypi.org/project/PrettyTable/): For printing text tables or HTML table    
   
   ** Note ** : In this script I actually used 
   [oyaml](https://github.com/wimglenn/oyaml) as it preserves order in python dictionary
   
   For field description it will just generated random Lorem ipsum one liner text using python library *loremipsum*   

# Usage 

```bash
> python yaml_to_table.py -h
usage: yaml_to_table.py [-h] --inputFile INPUTFILE [--out {txt,html,text}]

YAML file to (HTML) table converter

optional arguments:
  -h, --help            show this help message and exit
  --inputFile INPUTFILE
                        input yaml file to process
  --out {txt,html,text}
                        convert yaml to text table or html table

text table will be printed as STDOUT - html table will be save in html file
```

# Sample 

Let's take an example of simple kubernetes deployment file (as show below)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```
If you run script with this command :

```bash
python yaml_to_table.py --inputFile samples/k8sDeploy.yaml --out text
```

You will see output like this 

```bash
=> apiVersion:
+------------+---------------+----------+--------------+
| Field      | Example Value | Required | Description  |
+------------+---------------+----------+--------------+
| apiVersion | apps/v1       |          | Lorem ipsum. |
+------------+---------------+----------+--------------+
Raw yaml:
	apiVersion: apps/v1

=> kind:
+-------+---------------+----------+------------------------+
| Field | Example Value | Required | Description            |
+-------+---------------+----------+------------------------+
| kind  | Deployment    |          | Lorem ipsum dolor sit. |
+-------+---------------+----------+------------------------+
Raw yaml:
	kind: Deployment

=> metadata:
+--------+------------------+----------+--------------------------------------------------------------------------------------+
| Field  | Example Value    | Required | Description                                                                          |
+--------+------------------+----------+--------------------------------------------------------------------------------------+
| name   | nginx-deployment |          | Lorem ipsum dolor sit amet, consecteteur adipiscing elit b'nunc' b'id' b'eu'.        |
| labels |                  |          | Lorem ipsum dolor sit amet, consecteteur adipiscing elit b'duis' b'eu' b'id' b'sit'. |
|   app  | nginx            |          | Lorem ipsum dolor sit amet, consecteteur adipiscing.                                 |
+--------+------------------+----------+--------------------------------------------------------------------------------------+
Raw yaml:
	metadata:
	  name: nginx-deployment
	  labels:
	    app: nginx

=> spec:
+---------------------------+---------------+----------+---------------------------------------------------------------------------------------------------+
| Field                     | Example Value | Required | Description                                                                                       |
+---------------------------+---------------+----------+---------------------------------------------------------------------------------------------------+
| replicas                  | 3             |          | Lorem ipsum.                                                                                      |
| selector                  |               |          | Lorem ipsum.                                                                                      |
|   matchLabels             |               |          | Lorem ipsum dolor sit amet, consecteteur adipiscing elit b'arcu' b'id' b'ad' b'neque' b'a' b'eu'. |
|     app                   | nginx         |          | Lorem ipsum.                                                                                      |
| template                  |               |          | Lorem ipsum dolor sit amet, consecteteur.                                                         |
|   metadata                |               |          | Lorem ipsum dolor sit amet.                                                                       |
|     labels                |               |          | Lorem ipsum dolor sit amet, consecteteur adipiscing.                                              |
|       app                 | nginx         |          | Lorem ipsum dolor sit amet, consecteteur adipiscing elit b'diam' b'et'.                           |
|   spec                    |               |          | Lorem ipsum dolor sit.                                                                            |
|     containers            |               |          | Lorem ipsum dolor sit amet, consecteteur adipiscing elit b'quis'.                                 |
|         name              | nginx         |          | Lorem ipsum.                                                                                      |
|         image             | nginx:1.7.9   |          | Lorem ipsum dolor sit.                                                                            |
|         ports             |               |          | Lorem ipsum dolor sit amet, consecteteur.                                                         |
|             containerPort | 80            |          | Lorem ipsum dolor sit amet, consecteteur adipiscing elit.                                         |
+---------------------------+---------------+----------+---------------------------------------------------------------------------------------------------+
Raw yaml:
	spec:
	  replicas: 3
	  selector:
	    matchLabels:
	      app: nginx
	  template:
	    metadata:
	      labels:
	        app: nginx
	    spec:
	      containers:
	      - name: nginx
	        image: nginx:1.7.9
	        ports:
	        - containerPort: 80

```

If I need html as output then I can run it like this 

```bash
> python yaml_to_table.py --inputFile samples/k8sDeploy.yaml --out html
File samples/k8sDeploy.doc.html has been generated
```

Tool will generate output HTML file that will look like this :

![yaml to html](doc/k8s-html-out.png)


