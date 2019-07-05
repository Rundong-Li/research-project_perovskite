# research-project_perovskite
Jupyter notebooks to calculate the charge properties of pervoskite solar cells  

Based on the [BANDs](https://doi.org/10.1021/acs.jpclett.8b01446) model and bright/dark electron model

Now we should consider a comprenhensive model that describes the trapping and photoluminescence (*PL*). 

The energy dependent capture radius by Coulomb potential is given by 
$$
r_{c}(E)=\frac{e^{2}}{4\pi\varepsilon\varepsilon_{0}E} \tag{1}
$$
The Coulomb capture volume is $$V_{c}(E)=\frac{4}{3}\pi(r_{c}(E))^{3} \tag{2}$$ Volume of one defect is $$V_{d} = \frac{1}{N_{d}} \tag{3}$$ Fraction of the volume outside the Coulomb capture region that is contributing to *PL* $$V_{pl}(E)=1-\frac{4}{3}\pi N_{d}(r_{c}(E))^{3} \tag{4}$$ or $$V_{pl}(E)=1-DN_{d}/E^{3} \tag{5}$$ where $$D=\frac{4}{3}\pi \left(\frac{e^{2}}{4\pi\varepsilon\varepsilon_{0}}\right)^{3} \tag{6}$$ 
Concentration of photoexcited electrons (holes)is related to the quasi Fermi energy, $E_{Fn}(E_{Fp})$

The Fermi-Dirac distribution function is $$f_{n}(z)=\frac{1}{1+exp[z-\eta_{Fn}]}\tag{7}$$ 

where $\eta_{Fn}=E_{Fn}/k_{B}T \qquad z=E/k_{B}T$      

Quasi-Fermi energy is related to electron concentration:$$n_{total}=N_{cg}F_{1/2}(\eta_{Fn})$$     

where $N_{cg}=5.44\times10^{15}\times\left( T\cdot\frac{m^{*}}{m_{0}} \right)^{3/2}$ is the the conduction band density of states for perovskite (for perovskite $m^{*}/m_{0}=0.2$), and $F_{1/2}(\eta_{Fn})$ is the 

Fermi-Dirac integral: $\int_{0}^{+\infty}\frac{z^{1/2}}{1+exp(z-\eta_{Fn})}dz$ 

Then $\eta_{Fn}$ can be solved by:
$$\frac{n_{total}}{N_{cg}}=\int_{0}^{+\infty}\frac{z^{1/2}}{1+exp(z-\eta_{Fn})}dz \tag{8}$$    

The bright electrons (participate in *PL*) relative amount is given by $$nf_{pl}=\int_{z_{0}}^{+\infty}z^{1/2}f_{n}(z)V_{pl}(z)dz \tag{9}$$ The dark electrons relative amount is given by $$nf_{dr}=\int_{0}^{z_{0}}z^{1/2}f_{n}(z)dz+\int_{z_{0}}^{+\infty}z^{1/2}f_{n}(z)(1-V_{pl}(z))dz \tag{10}$$  And the total electrons relative amonut is given by $$nf_{total}=\int_{0}^{+\infty}z^{1/2}f_{n}(z)dz  \tag{11}$$  
Here $z_{0}$ is the cutoff energy and is defined by $V_{pl}(z_{0})=0$.   If $z<z_{0}$, then $V_{pl}<0$, which is unphysical, so we set $V_{pl}=0,1-V_{pl}=1$ and have equation$(9)$ and $(10)$        

Since we know the bright ($n$) and dark electrons ($n_{dr}$) proportion, in an equilibrium, we can calculate $\frac{n}{n_{total}}$,denoted as $A$. (Here $n_{total}=n+n_{dr}$)

$$n+N_{d}\rightleftharpoons n_{dr}$$
$$\frac{dn}{dt}=-k_{1}n+k_{-1}n_{dr}$$      
when equilibrium, $\frac{n}{n_{dr}}=\frac{A}{1-A},\frac{dn}{dt}=0.\therefore k_{-1}=\frac{A}{1-A}k_{1}\quad and \quad \frac{dn}{dt}=-k_{1}\left(n-\frac{A}{1-A}n_{dr}\right)$        

Here, $N_{d}$ is considered as a constant ($\because N_{d}\gg n_{dr}$), so $k_{1}$ is actually $k_{1}(N_{d}-n_{dr})$.

There is also non-radiative recombination between holes and electrons
$$\frac{dn_{br}}{dt}=-\int_{0}^{+\infty}RN_{R}N_{cg}V_{pl}(z)z^{1/2}f_{n}(z)dz  \tag{12}$$

$$\frac{dn_{dr}}{dt}=-\int_{0}^{+\infty}RN_{R}N_{cg}[1-V_{pl}(z)]^{2}z^{1/2}f_{n}(z)dz  \tag{13}$$

where $R$ is a rate constant, $N_{R}$ is the concentration of non-radiative recombination centers,

$N_{cg}=5.44\times10^{15}(T\times{m^{*}}/{m_{0}})^{3/2}$

Also, there is an Auger recombination for bright electrons, and a constant $k_{3}$

Now, the diffential equations can be written as:$$\frac{\partial n}{\partial t} = G(only\ at\  x=0)+D_{br}\frac{\partial^{2}n}{\partial x^{2}}-k_{1}\left(N_{d}-n_{dr}\right)\left(n-\frac{A}{1-A}n_{dr}\right)-k_{2}n^{2}-k_{3}n^{3}-\int_{0}^{+\infty}RN_{R}N_{cg}V_{pl}(z)z^{1/2}f_{n}(z)dz \tag{14}$$

$$\frac{\partial n_{dr}}{\partial t} = D_{dr}\frac{\partial^{2}n}{\partial x^{2}}+k_{1}\left(N_{d}-n_{dr}\right)\left(n-\frac{A}{1-A}n_{dr}\right)-\int_{0}^{+\infty}RN_{R}N_{cg}[1-V_{pl}(z)]^{2}z^{1/2}f_{n}(z)dz \tag{15}$$

In this notebook, I perform the calculation under room temperature(*i.e.* T = 300K), and since I use a extremely large 
$G$, which brings *non-degenerate case*, 

I use the **full** Fermi-Dirac Distribution 
function $equation \ (7)$, and if $n_{total}$ changes, I need to calculate for a new $\eta_{Fn}$ using $equation \ (8)$

Therefore, at a certain $\eta_{Fn}$, $$A=\frac{\int_{0}^{+\infty}\frac{z^{1/2}}{1+exp(z-\eta_{Fn})}V_{pl}(z)dz}{\int_{0}^{+\infty}\frac{z^{1/2}}{1+exp(z-\eta_{Fn})}dz} \tag{16}$$   

A is dependent on $\eta_{Fn}$(or say, $n_{total}$). Therefore, we should recalculate A every step using the new $n_{total}$

And $$V_{pl}(E)=1-\frac{4}{3}\pi N_{d}(r_{c}(E))^{3}=1-\frac{4}{3}\pi \left(\frac{e^{2}}{4\pi\varepsilon\varepsilon_{0}}\right)^{3}\frac{N_{d}}{E^{3}}=1-\frac{2.1431\times10^{-6}eV^{3}}{E^{3}} \tag{17}$$

$$\because z=\frac{E}{k_{B}T}\ and\ T=300K,\quad\therefore V_{pl}(z)=1-\frac{0.12405}{z^{3}}$$

$$\because V_{pl}(z_{0})=0,\quad\therefore z_{0}=0.49873$$
