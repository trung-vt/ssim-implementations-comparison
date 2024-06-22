```sh
python -m venv venv
```

or

```sh
python3 -m venv venv
```



```sh
source venv/bin/activate
```

```sh
pip install -r requirements.txt
```


- [x] `scikit-image`
- [x] `pytorch-msssim` SSIM implementation (cpu)
- [x] `pytorch-msssim` SSIM implementation (cuda)
- [x] `pytorch-ignite` (cpu)
- [x] `pytorch-ignite` (cuda)
- [x] `pytorch-msssim` MS-SSIM implementation (cpu)
- [x] `pytorch-msssim` MS-SSIM implementation (cuda)
- [ ] `tensorflow` SSIM
- [ ] `tensorflow` MS-SSIM


```
sigma  | ski         | pm_ssim_cpu  | pm_ssim_cuda  | ignite_cpu  | ignite_cuda   | pm_msssim_cpu   | pm_msssim_cuda
--------------------------------------------------------------------------------------------------------------------
0      | 1.0         | 1.0          | 1.0           | 1.0         | 1.0           | 1.0             | 1.0
10     | 0.932475    | 0.932476     | 0.925051      | 0.93338     | 0.93338       | 0.991116        | 0.989004
20     | 0.785769    | 0.78577      | 0.773435      | 0.788488    | 0.788487      | 0.967452        | 0.963898
30     | 0.637239    | 0.637241     | 0.62326       | 0.64153     | 0.641529      | 0.934444        | 0.929061
40     | 0.515662    | 0.515664     | 0.502339      | 0.521009    | 0.521008      | 0.897658        | 0.890179
50     | 0.422473    | 0.422474     | 0.410533      | 0.428363    | 0.428362      | 0.858975        | 0.850126
60     | 0.350911    | 0.350912     | 0.340449      | 0.357038    | 0.357037      | 0.821748        | 0.811729
70     | 0.295807    | 0.295808     | 0.286809      | 0.301982    | 0.301982      | 0.784397        | 0.772992
80     | 0.253616    | 0.253617     | 0.245654      | 0.259705    | 0.259704      | 0.748474        | 0.736515
90     | 0.219286    | 0.219287     | 0.212196      | 0.225221    | 0.225221      | 0.716415        | 0.703794
100    | 0.192074    | 0.192075     | 0.185719      | 0.197837    | 0.197837      | 0.684395        | 0.671093
```

Using `scikit-image` as the standard, only `pytorch-msssim` SSIM implementation running on the CPU produces the same results.

CPU and CUDA usually produce different results, except `pytorch-ignite`.

MS-SSIM is on a different scale.


```
sigma   | ski (ms)    | pm_ssim_cpu (ms)  | pm_ssim_cuda (ms)   | ignite_cpu (ms)   | ignite_cuda (ms)   | pm_msssim_cpu (ms)   | pm_msssim_cuda (ms)
-----------------------------------------------------------------------------------------------------------------------------------------------------
0       | 70.169756   | 18.487931         | 1.770556            | 30.798527         | 0.976009           | 24.499149            | 6.143879
10      | 70.015085   | 17.736758         | 1.78089             | 29.635338         | 0.983979           | 25.055882            | 6.077118
20      | 68.490763   | 18.464452         | 1.761029            | 29.015168         | 0.97937            | 24.244493            | 6.157712
30      | 69.294481   | 17.223086         | 1.758968            | 30.276556         | 0.98076            | 24.893183            | 6.232932
40      | 68.621014   | 16.835908         | 1.758389            | 27.313126         | 0.980421           | 24.674724            | 6.118175
50      | 68.212535   | 18.334048         | 1.76852             | 32.100331         | 0.976683           | 24.643792            | 6.093109
60      | 69.911158   | 17.54367          | 1.770424            | 30.590572         | 0.979468           | 24.570827            | 6.080821
70      | 69.408867   | 18.364719         | 1.762365            | 30.008297         | 0.981872           | 24.332108            | 6.087167
80      | 68.718978   | 17.624662         | 1.754644            | 31.902542         | 0.98296            | 24.42374             | 6.080809
90      | 67.992766   | 17.99126          | 1.774627            | 31.246952         | 0.982845           | 25.032125            | 6.099255
100     | 69.411668   | 17.365801         | 1.756611            | 32.058589         | 0.97857            | 24.913905            | 6.109575
```     

In terms of speed,

- CUDA:
`ignite_cuda` > 
`pm_ssim_cuda` > 
`pm_msssim_cuda`

- CPU:
`pm_ssim_cpu` > 
`pm_mssim_cpu` >
`ignite_cpu` > 
`ski`

`scikit-image` is the only one that uses `numpy.ndarray` instead of `torch.Tensor`.

`ignite_cuda` is faster yet "less demanding" on the GPU than `pm_ssim_cuda`.

# References

[ssim from `scikit-image`](https://github.com/scikit-image/scikit-image/blob/main/skimage/metrics/_structural_similarity.py)

[ssim from `pytorch-ignite`](https://github.com/pytorch/ignite/blob/master/ignite/metrics/ssim.py)

[ms & ms-ssim from `pytorch-msssim`](https://github.com/VainF/pytorch-msssim)

[ssim & ms-ssim from `tensorflow`](https://github.com/tensorflow/tensorflow/blob/v2.1.0/tensorflow/python/ops/image_ops_impl.py#L3314-L3438) 

[https://github.com/jorge-pessoa/pytorch-msssim](https://github.com/jorge-pessoa/pytorch-msssim)  

[https://ece.uwaterloo.ca/~z70wang/research/ssim/](https://ece.uwaterloo.ca/~z70wang/research/ssim/)  

[https://ece.uwaterloo.ca/~z70wang/publications/msssim.pdf](https://ece.uwaterloo.ca/~z70wang/publications/msssim.pdf)  

[Matlab Code](https://ece.uwaterloo.ca/~z70wang/research/iwssim/)   