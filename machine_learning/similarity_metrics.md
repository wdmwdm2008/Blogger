## 1. Euclidean distance

```Python
def get_distance(data1, data2):
    points = zip(data1, data2)
    diffs_squared_distance = [pow(a - b, 2) for (a, b) in points]
    return math.sqrt(sum(diffs_squared_distance)) 
```
    
## 2. cosine distance 
``` Python
def cosin_distance(vector1, vector2):
    dot_product = 0.0
    normA = 0.0
    normB = 0.0
    for a, b in zip(vector1, vector2):
        dot_product += a * b
        normA += a ** 2
        normB += b ** 2
    if normA == 0.0 or normB == 0.0:
        return None
    else:
        return dot_product / ((normA * normB) ** 0.5)
```

## 3. cosine distance using numpy

``` Python
sim = user_item_matric.dot(user_item_matric.T)
norms = np.array([np.sqrt(np.diagonal(sim))])
user_similarity=(sim / norms / norms.T)
```

## 4. cosine distance using scikit cosine_similarity

``` Python
from sklearn.metrics.pairwise import cosine_similarity
user_similarity=cosine_similarity(user_tag_matric)
```

## 5. cosine distance using scikit pairwise_distances

``` Python
from sklearn.metrics.pairwise import pairwise_distances
user_similarity = pairwise_distances(user_tag_matric, metric='cosine')
```
**tips: pairwise_distance = 1 - cosine_distance**


## 6. Manhattan_Distance

``` Python
def Manhattan(vec1, vec2):
    npvec1, npvec2 = np.array(vec1), np.array(vec2)
    return np.abs(npvec1-npvec2).sum()
```


## 7. Chebyshev_Distance

``` Python
def Chebyshev(vec1, vec2):
    npvec1, npvec2 = np.array(vec1), np.array(vec2)
    return max(np.abs(npvec1-npvec2))
```


## 8. Standardized Euclidean distance
http://blog.csdn.net/jinzhichaoshuiping/article/details/51019473
``` Python
def Standardized_Euclidean(vec1,vec2,v):
    from scipy import spatial
    npvec = np.array([np.array(vec1), np.array(vec2)])
    return spatial.distance.pdist(npvec, 'seuclidean', V=None)
```


## 9. Edit_Distance (Levenshtein distance)

``` Python
def Edit_distance_str(str1, str2):
    import Levenshtein
    edit_distance_distance = Levenshtein.distance(str1, str2)
    similarity = 1-(edit_distance_distance/max(len(str1), len(str2)))
    return {'Distance': edit_distance_distance, 'Similarity': similarity}
```

## Reference
https://blog.csdn.net/u010412858/article/details/60467382
