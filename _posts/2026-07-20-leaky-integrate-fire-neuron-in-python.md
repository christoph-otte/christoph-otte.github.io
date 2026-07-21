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

    def simulate(self, t, I_input, sigma=0):
        t = torch.as_tensor(t, dtype=torch.float64, device=self.device)
        I_input = torch.as_tensor(I_input, dtype=torch.float64, device=self.device)
        
        # Handle scalar input: expand to array
        if I_input.dim() == 0:
            I_input = I_input.expand(len(t))
        
        dt = t[1] - t[0]

        V = torch.zeros(len(t), dtype=torch.float64, device=self.device)
        V[0] = self.E_L
        spike_times = []
        noise = torch.randn(len(t), dtype=torch.float64, device=self.device)

        # Simulate the neuron dynamics using the Euler method
        for i in range(len(t) - 1):
            dV = (dt / self.tau_m) * (-(V[i] - self.E_L) + self.R_m * I_input[i]) + (self.R_m * sigma / self.tau_m) * torch.sqrt(dt) * noise[i]
            V[i+1] = V[i] + dV
            if V[i+1] >= self.V_th:
                spike_times.append(t[i].item())
                V[i+1] = self.V_reset

        # Calculate firing rate 
        T = t[-1] - t[0]
        return len(spike_times) / T.item(), V.cpu().numpy(), spike_times
```