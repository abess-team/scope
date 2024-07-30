:parenttoc: True

Various Iterative-Algorithms Support
=======================================

Implemented Solvers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``skscope`` provides several solvers, each with a similar interface, to solve sparsity-constrained optimization problems: 

.. math::

    \arg\min\limits_{\theta \in R^p} f(\theta) \text{ s.t. } ||\theta||_0 \leq s. 

Here is a list of the currently available solvers:

- ``GraspSolver``: implements the Gradient Support Pursuit (GraSP) algorithm that generalizes the Compressive Sampling Matching Pursuit (CoSaMP) algorithm `[1]`_.

- ``ScopeSolver``: implements the Sparse-Constrained Optimization sPlicing itEration (SCOPE) algorithm that generalizes the Adaptive BEst Subset Selection (ABESS) algorithm `[10]`_.

- ``HTPSolver``: implements the hard thresholding pursuit (HTP) algorithm `[2]`_ `[3]`_. 

- ``IHTSolver``: implements the iterative hard thresholding (IHT) algorithm `[4]`_ `[5]`_. 

- ``FobaSolver``: implements Forward-Backward greedy algorithm `[6]`_.

- ``OMPSolver``: implements Orthogonal Matching Pursuit (OMP) algorithm `[7]`_ `[8]`_. 

- ``ForwardSolver``: implements forward stepwise algorithm in statistics literature `[9]`_. 


The details of ``ForwardSolver`` and ``OMPSolver``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``OMPSolver`` and ``ForwardSolver`` are similar but not identical, although both methods solve SCO by sequentially adding a variable of the highest importance. Their difference lies in the mathematical definition of "importance".

- For ``OMPSolver``, the importance is defined as:

  .. math::

    \arg\max_{j\in \{1, \ldots, p\}}|\nabla_{\boldsymbol{\theta}_j} f(\boldsymbol{\theta}^{(t)})|

  where :math:`\boldsymbol{\theta}^{(t)}` is the current estimated parameter.

- As for ``ForwardSolver``, the importance is measured by:

  .. math::

    \arg\max_{j\in \{1, \ldots, p\}} \big( f(\boldsymbol{\theta}^{(t)}) - \min_{ \left\{ \boldsymbol{\theta} \in \mathbb{R}^p:  \textup{supp}(\boldsymbol{\theta}) = \{j\} \cup \mathcal{A}^{(t)} \right\} } f(\boldsymbol{\theta}) \big)

  where :math:`\mathcal{A}^{(t)}` is the support set of :math:`\boldsymbol{\theta}^{(t)}`.

We can see that computing the importance in ``ForwardSolver`` involves more intensive computations since it has to solve the optimization problem:

.. math::

  \min_{ \{ \boldsymbol{\theta}: \textup{supp}(\boldsymbol{\theta}) = \mathcal{A}^{(t)} \cup \{j\} \} } f(\boldsymbol{\theta}).

for each :math:`j \in \{1, \ldots, p\}`. On the other hand, the criterion used in ``OMPSolver`` can be considered as a first-order approximation of that of ``ForwardSolver`` and it can be computed with a much cheaper cost. Yet, we can expect that ``ForwardSolver`` would lead to a better statistical performance than ``OMPSolver``.

Finally, for the choice between ``OMPSolver`` and ``ForwardSolver``, we provide the following guideline:

- If `p` is small (e.g., less than 10), ``ForwardSolver`` is tempting in small-sample regimes; otherwise, the first choice should be ``OMPSolver``.





Reference
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- _`[1]`: Bahmani, S., Raj, B., & Boufounos, P. T. (2013). Greedy sparsity-constrained optimization. The Journal of Machine Learning Research, 14(1), 807-841.

- _`[2]` Foucart, S. (2011). Hard thresholding pursuit: an algorithm for compressive sensing. SIAM Journal on numerical analysis, 49(6), 2543-2563.

- _`[3]` Yuan, X. T., Li, P., & Zhang, T. (2017). Gradient Hard Thresholding Pursuit. J. Mach. Learn. Res., 18(1), 6027-6069.

- _`[4]` Blumensath, T., & Davies, M. E. (2009). Iterative hard thresholding for compressed sensing. Applied and computational harmonic analysis, 27(3), 265-274.

- _`[5]` Jain, P., Tewari, A., & Kar, P. (2014). On iterative hard thresholding methods for high-dimensional m-estimation. Advances in neural information processing systems, 27.

- _`[6]` Liu, J., Ye, J., & Fujimaki, R. (2014). Forward-backward greedy algorithms for general convex smooth functions over a cardinality constraint. In International Conference on Machine Learning (pp. 503-511). PMLR.

- _`[7]` Wang, J., Kwon, S., & Shim, B. (2012). Generalized orthogonal matching pursuit. IEEE Transactions on signal processing, 60(12), 6202-6216.

- _`[8]` Tropp, J. A., & Gilbert, A. C. (2007). Signal recovery from random measurements via orthogonal matching pursuit. IEEE Transactions on information theory, 53(12), 4655-4666.

- _`[9]` Hastie, T., Tibshirani, R., Friedman, J. H., & Friedman, J. H. (2009). The elements of statistical learning: data mining, inference, and prediction (Vol. 2, pp. 1-758). New York: springer.

- _`[10]` Zhu, J., Wen, C., Zhu, J., Zhang, H., & Wang, X. (2020). A polynomial algorithm for best-subset selection problem. Proceedings of the National Academy of Sciences, 117(52), 33117-33123.