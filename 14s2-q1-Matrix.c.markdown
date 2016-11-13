COMP1927 14s2 Question 1
========================

Jashank Jeremy <jashankj@cse.unsw.edu.au>

> Your task for this question is to implement the `multiply()`
> function, which is currently incomplete.  For two matrices _X_ (with
> dimensions _M×N_) and _Y_ (with dimensions _N×P_), multiplication
> produces a matrix _Z_ (with dimensions _M×P_) whose elements are
> defined as follows:
>
>     Z=X*Y, where Z[i,j] = Sum{k=0..N-1}(X[i][k] * Y[k][j])
>
> The `multiply()` function takes two matrices as parameters, checks
> their compatibility, and then multiples the two matrices, storing
> the result in a newly-created matrix, and returning the new matrix
> as the result.
>
> The overall structure of the `multiply()` function is somewhat like
> the structure of the existing `add()` function. But note that the
> compatibility check for matrix multiplication is different to the
> compatibility check for matrix addition. Note also that all output,
> including error messages, should be written to the standard output.

A good place to start looking, then, is the existing `add()` function:

``` c
// add two MxN matrices; return new MxN matrix
// returns NULL if matrices are not compatible
Matrix add (Matrix x, Matrix y) {
    // check validity/compatibility of matrices
    assert (x != NULL && x->val != NULL);
    assert (y != NULL && y->val != NULL);
    if (x->nrows != y->nrows || x->ncols != y->ncols) {
        printf ("Incompatible matrices\n");
        return NULL;
    }
    // create output matrix of appropriate dimensions
    int i, j;
    Matrix z;
    z = newMatrix (x->nrows, x->ncols);
    // fill output matrix
    for (i = 0; i < x->nrows; i++) {
        for (j = 0; j < x->ncols; j++) {
            z->val[i][j] = x->val[i][j] + y->val[i][j];
        }
    }
    return z;
}
```

So, let's begin:

``` c
// multiply MxN and NxP matrices; return new MxP matrix
// returns NULL if matrices are not compatible
Matrix multiply (Matrix x, Matrix y) {
    assert (x != NULL && x->val != NULL);
    assert (y != NULL && y->val != NULL);
```

We check the compatibility of the two matrices, and bail out if
they're not compatible.  I choose _puts(3)_ over _printf(3)_, but
there's no particular reason to do so.

``` c
    if (x->ncols != y->nrows) {
        puts ("Incompatible matrices");
        return NULL;
    }
```

Next, we create our result matrix, `z`.  We can, and should, use the
`newMatrix` function, because it abstracts away the heavy lifting we'd
otherwise have to do.

``` c
    Matrix z = newMatrix (x->nrows, y->ncols);
```

And now, we have the same pair of inner `for`-loops over `i` and `j`,
the number of rows and columns in our result matrix, respectively.

Given that new matrices are filled with zeros when we create them, we
can have a `for` loop over the inner sum from _k = 0_ to _N_ adding to
_Z[i,j]_ the multiple of _X[i,k]_ and _Y[k,j]_.

If you prefer, you can have more braces.  I've omitted them here.

The 2014s2 exam did not use ISO C99 (as you'd enable with `-std=c99`),
so you had to declare `i`, `j`, and `k` before using them in the `for`
loop.  I consider this to be bad style.

``` c
    for (int i = 0; i < x->nrows; i++)
        for (int j = 0; j < y->ncols; j++)
            for (int k = 0; k < x->ncols; k++)
                z->val[i][j] += x->val[i][k] * y->val[k][j];
```

Well, that's it.

``` c
    return z;
}
```
