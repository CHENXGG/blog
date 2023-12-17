> TIME：2023.12.13，Wednesday，🌥️

# CHRIP波形与脉冲压缩技术

Chrip信号（**L**inear **F**requency **M**odulation Signal, LFM）是声呐的常用信号。相比于连续CW信号，他有着更大的带宽，能够提高声呐的距离分辨力。

在声呐定位中存在一组矛盾：作用距离与距离分辨力之间的矛盾。在声呐定位时如果要能够分辨两个目标需要使得两个目标的回波信号不发生重叠，根据反射定理我们很容易推断出距离分辨力为$\displaystyle\Delta r = \frac{c}{2B} = \frac{c\tau}{2}$。也就是说如果需要得到较优的距离分辨力，我们希望信号的持续时间$\tau$尽可能短，但是这回导致信号的能量降低。显然，信号能量的降低会影响到信号的作用距离。

而Chrip信号能够实现在相同的持续时间$\tau$下，信号的带宽增大（**拓频**）。同时，在回波信号经过匹配滤波后，信号的脉冲宽度将会大大缩短（**脉冲压缩**）。这使得我们得到了一种同时具备宽带与窄脉的技术。

# CHRIP波形

Chrip信号作为一种线性调频信号，它的表达式为
$$
x(t) = \frac{1}{\sqrt{T}}\mathrm{rect}(\frac{t}{T})\mathrm{cos}(2\pi f_0 + \pi \mu t^2) \tag 1
$$
则该信号的瞬时相位与瞬时频率为
$$
\begin{cases}
\displaystyle \theta(t) = 2\pi f_0 + \pi \mu t^2  \\
\displaystyle f(t) = \frac{d\theta}{dt}/2\pi = f_0+\mu t
\end{cases} \tag 2
$$
则显然信号带宽$B\approx F = \mu T$

则复信号定义为
$$
s(t) = \frac{1}{\sqrt{T}}\mathrm{rect}(\frac{t}{T})e^{j(2\pi f_0 + \pi \mu t^2)} \tag 3
$$
复包络信号定义为
$$
u(t) = \frac{1}{\sqrt{T}}\mathrm{rect}(\frac{t}{T})e^{j(\pi \mu t^2)} \tag 4
$$
根据[复包络的性质](public_docs/dsp/sonar_signal_processing/声呐系统介绍.md)我们可以得知，原信号的频谱形状与复包络一致。

因此我们根据复包络计算频谱
$$
\begin{aligned}
 U(f) & = \int_{-\infin}^{\infin} u(t)e^{-j2\pi ft}dt \\
	 & = \int_{-\infin}^{\infin} \frac{1}{\sqrt{T}}\mathrm{rect}(\frac{t}{T})e^{j(\pi \mu t^2)}e^{-j2\pi ft}dt \\
	 & = \int_{-\frac{T}{2}}^{\frac{T}{2}} \frac{1}{\sqrt{T}}e^{-j\pi(2ft-\mu t^2)}dt \\
	 & = \int_{-\frac{T}{2}}^{\frac{T}{2}} \frac{1}{\sqrt{T}}e^{-j\pi\frac{f^2}{\mu}[1-(\frac{\mu}{f}t-1)^2]}dt \\
	 & = \frac{1}{\sqrt{T}}e^{j\pi \frac{f^2}{\mu}} \int_{-\frac{T}{2}}^{\frac{T}{2}}e^{j\frac{\pi}{2}2\mu(t-\frac{f}{\mu})^2}dt
\end{aligned} \tag 5
$$
令$\displaystyle x=\sqrt{2\mu}(t-\frac{f}{\mu})， v_1 = \sqrt{2\mu}(\frac{T}{2}-\frac{f}{\mu})， v_2 = \sqrt{2\mu}(\frac{T}{2}+\frac{f}{\mu})$，式$(5)$改写为
$$
\begin{aligned}
U(f) & = \frac{1}{\sqrt{2\mu T}}e^{-j\pi \frac{f^2}{\mu}} \int_{-v_2}^{v_1}e^{j\pi x^2}dx \\
	 & = \frac{1}{\sqrt{2\mu T}}e^{-j\pi \frac{f^2}{\mu}} \int_{-v_2}^{v_1}[\mathrm{cos}(\frac{\pi}{2} x^2)+j\mathrm{sin}(\frac{\pi}{2} x^2)]dx \\
	 & = \frac{1}{\sqrt{2\mu T}}e^{-j\pi \frac{f^2}{\mu}}\{ [c(v_1)+c(v_2)]+j[s(v_1)+s(v_2)]\}
\end{aligned} \tag 6
$$
其中，$c(\cdot)$和$s(\cdot)$分别式菲涅尔积分公式
$$
c(v) = \int_0^{v}\mathrm{cos}(\frac{\pi x^2}{2})dx, \quad\quad s(v) = \int_0^{v}\mathrm{sin}(\frac{\pi x^2}{2})dx \tag 7
$$
根据其性质可知，在$BT \gg 1$时，95%的能量集中在$\displaystyle \left[-\frac{B}{2}, \frac{B}{2}\right]$中，频谱近似为矩形。近似得到
$$
U(f) \approx \frac{1}{\sqrt{2\mu T}}e^{j[-\frac{\pi f^2}{\mu}+\frac{\pi}{4}]}, \quad \quad \vert f\vert \le \frac{B}{2} \tag 8
$$




