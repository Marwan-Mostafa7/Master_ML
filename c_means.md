## C means algorithm

```python
m = 2 ### let fuzziness factor = 2
X = np.array([
    [1, 2],
    [3, 4]
])
C = np.array([0.01, 2.4, 3.2])
```

### Creating Distance matrix
Distance between each point in matrix and each Center
|x - c|

```python
for i, x in enumerate(X.flatten()):
    for j, c in enumerate(C):
        D[j][i] = abs(x - c)
```

        
### Membership matrix

```python
for c in range(D.shape[1]):
    a= []
    for d in D[:, c]:
        s = 0
        for i, _ in enumerate(D[:, 0]):
            s += ((d**m) / D[i, 0]**m)
        a.append(s)
    norm = [round(float(i)/sum(a), 2) for i in a]
    Mio[:, c] = norm
    
## Output
array([[0.84, 0.93, 0.05, 0.03],
       [0.08, 0.04, 0.48, 0.48],
       [0.08, 0.03, 0.47, 0.48]])
```


### Updating Centers

```python
for r in range(Mio.shape[0]):
    c_up = sum(X.flatten()* Mio[r])
    c_down = sum(Mio[r])
    c = c_up / c_down
    C_new[r] = round(c, 2)
C_new


## Output
array([2.95, 2.11, 1.38])

```


### Functional coding


```python
m = 2 ### let fuzziness factor = 2
X = np.array([
    [1, 2],
    [3, 4]])

C = np.array([0.01, 2.4, 3.2])

max_margin = 1

best_Mio, best_centers = 0, 0

for i in range(10):
    
    D = calculate_distance(X, C)
    
    Mio = update_membership(D)
    
    C = update_centers(Mio)
    
    margin = np.matrix(Mio).max()
    
    if margin < max_margin:
        max_margin = margin
        best_centers = C
        best_Mio = Mio
        
    if i % 2 == 0:
        print(f"iteration: {i}")
        print(f"Best Margin = {max_margin}")
        print("Centers")
        print(C)
        print("=================")
        
        
### Output

iteration: 0
Best Margin = 0.96
Centers
[2.95 2.11 1.38]
=================
iteration: 2
Best Margin = 0.77
Centers
[3.31 2.02 1.73]
=================
iteration: 4
Best Margin = 0.77
Centers
[3.38 1.78 1.74]
=================
iteration: 6
Best Margin = 0.77
Centers
[3.39 1.73 1.74]
=================
iteration: 8
Best Margin = 0.77
Centers
[3.39 1.73 1.74]
=================
```

### Best Membership matrix

```python
best_Mio

array([[0.84, 0.93, 0.05, 0.03],
       [0.08, 0.04, 0.48, 0.48],
       [0.08, 0.03, 0.47, 0.48]])

```

#### Such Membership indicates that

X = 1 belong to Cluster C_1 with 85\% 

X = 2 belong to Cluster C_2 with 93\% 

X = 3 belong to Cluster C_3 with 48\% 

X = 4 belong to Cluster C_3, C_4 with 48\% 
