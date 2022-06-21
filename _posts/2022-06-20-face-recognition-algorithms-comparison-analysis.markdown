---
layout: post
title: "Face Recognition on ORL database. Algorithms implementation and analysis."
excerpt: "ORL is a simple database of human faces. It is a great collection of data to test some recognition algorithms on and analyse their performance."
---

The [ORL face-recognition](https://github.com/lucianodainic/face-recognition) project comprises several machine learning algorithms on different levels of complexity. In order to keep up with their following description we need some notations and prior explanation:

- ORL database contains 10 photos of 40 distinct persons.
- The size of each image is 92x112 pixels, with 256 grey levels per pixel.
- $ A \in \mathbb{R}^{m,n} $ is the training matrix, where _m_ = 1034 (the vectorized image) and _n_ = 40*_no._ of photos from a class allocated for training (first 6, 8 or 9).
- For better results, during testing step, use last (4, 2 or 1) remaining photos.
- Beside the algorithm implementation .py files contain a function that run the statistics.
- Statistics are computed for the selected algorithm and database configuration.
- All the plots are displayed in the GUI after computing, also .png images can be found in _static/images/statistics/_ folder.
- No scikit-learn or OpenCV built-ins for algorithms implementation were used.

## _k_-Nearest Neighbors

_k_-NN is arguably the simplest machine learning algorithm. It aims to find the closest _k_ neighbors to a given data point _x_ and tell its class membership.  So, i used the _k_-NN concept to work on ORL database in order to check if the algorithm returns the similar person to that in the test photo.

The implementation has a straightforward approach: it calculates the distances between selected photo for testing and each column in _A_, then returns the index of the closest photo.
The distances are computed with different norms (click the [link](https://lucianodainic.github.io/2022/06/12/vector-norms.html) to learn more).
The code bellow is from _nn.py_ script and represents the implementation of _k-NN_ for _k=1_.

~~~python
    def run_alg(A,norm,test_img):
        z = np.zeros(shape = (1,len(A[0])))
        for i in range(len(A[0])):  
            if norm == 'n1':
                z[0,i] = la.norm(test_img-A[:, i], 1)
            elif norm == 'n2':
                z[0,i] = la.norm(test_img-A[:, i])
            elif norm == 'ninf':
                z[0,i] = la.norm(test_img-A[:, i], np.inf)
            elif norm == 'ncos':
                z[0, i] = 1 - np.dot(test_img, A[:, i]) / \
                (la.norm(test_img) * la.norm(A[:, i]))
        return np.argmin(z)
~~~

For proper _k-NN_ we need to select _k_ neighbors (usually 3,5,7,..). So, the following code, that can be found in _knn.py_ file, needs to be added:

~~~python
    def run_alg(A,norm,test_img,k,db_config):
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

## Eigenfaces

This algorithm is a bit more complex, but in the end it uses nearest neighbour concept as well.  
Eigenfaces needs a step of preprocessing prior to its actual execution, in order to prepare the "face space" (1) comprising the most relevant and descriptive _k_ eigenvectors (here 20, 40, 60, 80, 100), a concept known as PCA
(principal components analysis).

The proper eigenvectors are computed for the covariance matrix of the _A_ matrix. As covariance matrix is a square, symmetric matrix it will result with size _m x m_. The step of computing the eigenvectors will take a huge amount of time. Thus, we reduce their dimension to _n x n_. After that we project the _A_ matrix onto _L_ space and get the much desired eigenfaces, called so because eigenvectors, if plotted, look like phantoms faces. Once we got them, next step and final one will be creating the "face space" by extracting the first _k_ eigenvectors with greatest eigenvalues and projecting the _A_ matrix only on those.

~~~python
    def opt_preproc(A,k):
        ...
        # calculate the average photo
        medium = np.mean(A,axis=1)

        # subtract the resulted average photo from each photo within the training dataset 
        A = (A.T-medium).T

        # calculate covariance matrix of resulted matrix 
        cov_matrix = np.matmul(A.T,A)

        # compute eigenvectors and eigenvalues for the covariance matrix
        # d = vector of eigenvalues, L = matrix w/ eigenvectors
        d, L = la.eig(cov_matrix)
        
        # project the entire training dataset on the entire eigenvectors space
        L = np.matmul(A,L)

        # perform an ascending sorting of eigenvectors by their eigenvalues and extract the last k ones backwards
        L = L[:,np.argsort(d)[-k:]]

        # project A on face space
        projections = np.dot(A.T,L)
~~~

After preprocessing is done we can try some testing. The test image is required some alterations to be able to perform the _NN_ into the "face space".

~~~python
    def run(norm,test_img):
        test_img = test_img - medium
        test_img = np.matmul(test_img.T,L)
        test_img = test_img.T
        return nn.run_alg(projections.T,norm,test_img)
~~~

## Lanczos

Lanczos is the third and last used algorithm. It represents an iterative way of finding those "best" _k_ eigenvectors. The preprocessing step has the projection of _A_ on the "face space" as output and proper execution and testing uses nearest neighbour as well. The code function can be found at _scripts/lanczos.py_.

___References___

[(1)](https://www.cin.ufpe.br/~rps/Artigos/Face%20Recognition%20Using%20Eigenfaces.pdf) Turk, Matthew A., and Alex P. Pentland. "Face recognition using eigenfaces." Proceedings. 1991 IEEE computer society conference on computer vision and pattern recognition. IEEE Computer Society, 1991.

&nbsp;

__Want to leave a comment?__ Feel free to join the [Discussions](https://github.com/lucianodainic/face-recognition/discussions/1).
