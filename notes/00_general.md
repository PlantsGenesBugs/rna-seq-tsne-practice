### General changes and notes

`import seaborn as sns; sns.set()` - sns.set() deprecated and replaced with sns.set_theme()  
pickle module - allows "pickling" of objects to a form that can easily be saved to disk or transmitted over a network; allows saving and retrieving of complex data structures 

numpy - to work with numbers/arrays/general mathematics functions
pylab - for plotting (bulk imports matplotlib.pyplot and numpy)
seaborn - data visualisation
pandas - usage of data frames
scipy - algorithms for optimisation, integration, eigenvalue problems, algebraic and differential equations, other statistical functions
matplotlib - plotting data
sklearn - machine learning applications (including dimension reduction)

### Installing FIt-SNE:  
Downloaded FFTW (v 3.3.11) for OS from here: https://www.fftw.org/download.html  
Moved to root directory (necessary??)  
Go to root directory in Terminal  
Clone the FIt-SNE GitHub repository
Compile FIt-SNE according to github repo instructions: `g++ -std=c++11 -O3 src/sptree.cpp src/tsne.cpp src/nbodyfft.cpp -I ~/fftw-3.3.11/api/ -L ~/fftw-3.3.11/.libs/ -o bin/fast_tsne -pthread -lfftw3 -lm`

THIS DID NOT WORK. Other approach (use HomeBrew):

Install FFTW: 
brew install fftw 
brew --prefix fftw  (returns /opt/homebrew/opt/fftw)

Clone FIt-SNE repo:
cd ~
git clone https://github.com/KlugerLab/FIt-SNE.git
cd ~/FIt-SNE

Compile FIt-SNE, giving it the express directory of FFTW:
g++ -std=c++11 -O3 \
    src/sptree.cpp src/tsne.cpp src/nbodyfft.cpp \
    -I/opt/homebrew/opt/fftw/include \
    -L/opt/homebrew/opt/fftw/lib \
    -o bin/fast_tsne \
    -pthread -lfftw3 -lm \
    -Wno-address-of-packed-member
    
Warnings generated that might be consequential: 
`ld: warning: dylib (/opt/homebrew/opt/fftw/lib/libfftw3.dylib)
was built for newer macOS version (26.0) than being linked (11.1)`

Current MacOS version is 26.5.2, but FIt-SNE compilation is defaulting to an older deployment target. Recompile and specify correct MacOS version:

MACOSX_DEPLOYMENT_TARGET=26.0 g++ -std=c++11 -O3 \
    src/sptree.cpp src/tsne.cpp src/nbodyfft.cpp \
    -I/opt/homebrew/opt/fftw/include \
    -L/opt/homebrew/opt/fftw/lib \
    -o bin/fast_tsne \
    -pthread -lfftw3 -lm \
    -Wno-address-of-packed-member
    
To set directory of FIt-SNE package, I anonymised the directory path by importing os before importing sys, so I could use the `~` annotation along with os.path.expanduser().

Data loading: adjusted directory path.

Before running data loading block, take quick peek at data in terminal using head function.
`sample_heatmap_plot_data.csv` seems to contain cluster data, so must be an output from a PCA or other cluster analysis already performed.

head of the exon matrices spits out the whole file... I suppose it's all just one giant CSV?

%%time is a cool little command that measures how long the following cell takes to execute. Useful for code optimisation! (Another version is %time which times one statement/expression)

Javascript error encountered when trying to plot. Issue with Jupyter notebook backend. Fixed by adding %matplotlib inline at top of notebook. 

Changing np.matrix to np.asarray. Checked by running print(type(logCPM)).


