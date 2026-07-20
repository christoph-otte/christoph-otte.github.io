---
layout: post
title: Leaky Integrate-And-Fire Neuron in Python
description: This article reports on how I integrated the common Leaky Integrate-and-Fire model of neurons in Python.
summary: This article reports on how I integrated the common Leaky Integrate-and-Fire model of neurons in Python.
tags: [neuron]
math: true
---

The model depends on the following differential equation:

$$
\tau_m \frac{\mathrm{d}V(t)}{\mathrm{d}t} = -g_L\bigl(V(t)-E_L\bigr) + I_{\mathrm{ext}}(t)
$$

I started defining a new class:

```python
import torch

class LIFNeuron:
    def __init__(self, tau_m, E_L, V_th, V_reset, R_m):
        self.tau_m = tau_m
        self.E_L = E_L
        self.V_th = V_th
        self.V_reset = V_reset
        self.R_m = R_m
        self.device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
````