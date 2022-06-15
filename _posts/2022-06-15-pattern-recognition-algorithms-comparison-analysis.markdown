---
layout: post
title: "Pattern Recognition on ORL database. Algorithms implementation and analysis."
excerpt: "ORL is a simple database of human faces. It is a great collection of data to test some recognition algorithms on and analyse their performance."
---

The [ORL pattern-recognition](https://github.com/lucianodainic/pattern-recognition) project comprises several machine learning algorithms on different levels of complexity. In order to keep up with their following description we need some notations and prior explanation:

- ORL database contains 10 photos of 40 distinct persons.
- The size of each image is 92x112 pixels, with 256 grey levels per pixel.
- $ A \in \mathbb{R}^{m,n} $ is the training matrix, where _m_ = 1034 (the vectorized image) and _n_ = 40*_no._ of photos from a class allocated for training (first 6, 8 or 9).
- For better results, during testing step, use last (4, 2 or 1) remaining photos.
- Beside the algorithm implementation .py files contain a function that run the statistics.
- Statistics are computed for the selected algorithm and database configuration.
- All the plots are displayed in the GUI after computing, also .png images can be found in _static/images/statistics/_ folder.
- No scikit-learn or OpenCV built-ins were used.

## _k_-Nearest Neighbors

_k_-NN is arguably the simplest machine learning algorithm. It aims to find the closest _k_ neighbors to a given data point _x_ and tell its class membership.  So i used the _k_-NN concept to work on ORL database in order to check if the algorithm returns the similar person to that in the test photo.

The implementation has a straightforward approach: it calculates the distances between selected photo for testing and each column in _A_, then returns the index of the closest photo.
The distances are computed with different norms (click the [link](https://lucianodainic.github.io/2022/06/12/vector-norms.html) to learn more).
The code bellow is from _nn.py_ script and represents the implementation of _k-NN_ for _k=1_.

~~~python
    def run(A,norm,test_img):
        z = np.zeros(shape = (1,len(A[0])))
        for i in range(len(A[0])):  
            if norm == 'n1':
                z[0,i] = la.norm(test_img-A[:, i], 1)
            elif norm == 'n2':
                z[0,i] = la.norm(test_img-A[:, i])
            elif norm == 'ninf':
                z[0,i] = la.norm(test_img-A[:, i], np.inf)
            elif norm == 'ncos':
                z[0,i] = 1-(np.dot(test_img,A[:,i]))/(la.norm(test_img)*la.norm(A[:, i]))
        return np.argmin(z)
~~~

For proper _k-NN_ we need to select _k_ neighbors (usually 3,5,7,..). So, the following code, that can be found in _knn.py_ file, needs to be added:

~~~python
    def run(A,norm,test_img,k,db_config):
        ...
        # sort the distances and store coressponding indexes in pos
        pos = np.argsort(z)

        # for first k indexes in pos calculate each class membership.
        neighbs_class = np.zeros(shape=(1,k))
        for i in range(k):
            neighbs_class[0,i] = int(pos[0,i]/int(db_config[0]))

        # for each distinct class in the neighbs_class calculate the frequency.
        uniq, counts = np.unique(neighbs_class, return_counts=True)

        #  return first photo from the most occurring class.
        return uniq[np.argmax(counts)]*int(db_config[0])
~~~
