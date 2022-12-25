---
layout: post
title: 'Local wave activity budget correction'
comments: true
tags: ['research']
---

It came to our attention that one of the terms in the LWA budget as computed in [the currently published code on my GitHub](https://github.com/csyhuang/hn2016_falwa) may require correction. This concerns the meridional convergence of eddy momentum flux, which is currently evaluated in the displacement coordinate (Φ'), where it should really be evaluated in the real latitude (Φ). 

The discrepancy does not affect the zonal mean budget of wave activity, but it may cause spurious residual/sources/sinks locally if not corrected.

Noboru and I are currently assessing the error that this causes in our previous results and we will let you know as soon as we find out.

In the meantime, if you want to correct your diagnostic by yourself, the remedy is relatively simple (adding a correction term) -- please see [the write-up from Noboru on GitHub](https://github.com/csyhuang/hn2016_falwa/blob/master/notes/LWA-budget-revision-20221222.pdf). If you have specific questions or concerns, we'll be happy to help.

Our contact method:

- Noboru Nakamura nnn@uchicago.edu
- Clare Huang csyhuang@protonmail.com    