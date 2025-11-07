# Landslide-PSInSAR-SNAP-STAMPS
This repo contains some resources about remote sensing, and more detailed about PSInSAR. PSInSAR is a technique to assest surface deformation, can be use to detect landslide. This repo contains resources for PSInSAR performing with SNAP and STaMPS
## 1. Remote Sensing
### Books
[Fundamentals of Remote Sensing](https://natural-resources.canada.ca/sites/nrcan/files/earthsciences/pdf/resource/tutor/fundam/pdf/fundamentals_e.pdf)
### Courses
- There are some cources about remote sensing form NASA: [NASA ARSET](https://appliedsciences.nasa.gov/what-we-do/capacity-building/arset/arset-fundamentals)
- Courses about remote sensing and GIS from Edx. Below are specialization programs so you can not enroll for free. But click in the cources inside the specialization, you can audit them for free.
    - [Fundamentals of Geospatial Science](https://www.edx.org/certificates/professional-certificate/alaskax-fundamentals-of-geospatial-science?index=product&queryId=95a8ca0d8c1ac0d67e00450baf1f9fb5&position=3)
    - [Geographic Information Systems (GIS)](https://www.edx.org/masters/micromasters/dux-geographic-information-systems-gis?index=product&queryId=95a8ca0d8c1ac0d67e00450baf1f9fb5&position=2)
## 2. PSInSAR
### Books
- There are book in the PSInSAR folder:
    - The GMTSAR_theory.pdf is a file of the book "Satellite Radar Interferometry: Theory and Practice". From this book, you can learn detailed theory about SAR and InSAR. Especially the physical principles.
    - Another book is PHD thesis of Dr.  Andrew Hooper about PSInSAR.
### Courses
- A course about SAR from Edx: 
    - [Synthetic Aperture Radar (SAR) Applications](https://www.edx.org/certificates/professional-certificate/alaskax-synthetic-aperture-radar-sar-applications?index=product&queryId=95a8ca0d8c1ac0d67e00450baf1f9fb5&position=1)
- Also check NASA ARSET for more resouces: [NASA ARSET](https://appliedsciences.nasa.gov/what-we-do/capacity-building/arset/arset-fundamentals)
### Tutorials
- Prof Dinh Ho Tong Minh shares a lot of resources about PSInSAR, including tutorials for SNAP and STaMPS. Check his GitHub repo: [Dinh Ho Tong Minh](https://github.com/DinhHoTongMinh)
- He has many lectures about PSInSAR on YouTube. These are very helpful, you can watch the lecture for instruction and go to his GitHub repo for resouces used in the lecture.
    1. [Introduction to SAR Interferometry](https://youtu.be/sQXDCJrLHp4?si=pXXgPQHAZntonIB6)
    2. [Sentinel-1 InSAR processing workflow with SNAP, Session 1/3](https://youtu.be/5a5gBPA9Gbk?si=DZSVYcXBe0N7E9It)
    3. [SNAP2StaMPS – Data preparation for StaMPS PSInSAR processing with SNAP, Session 2/3](https://youtu.be/HzvvJoDE8ic?si=t70OJQK_LoC_RGUV)
    4. [Persistent Scatterer InSAR time series with StaMPS in Window and Unix, Session 3/3](https://youtu.be/a1WlsoRrlrU?si=VXTBZx50-yfDKdkQ)
    5. [Persistent Scatterer InSAR time series](https://youtu.be/TfM6IL3rjcY?si=dpz-o2IVaBO1t-MG)
- Join Prof Dinh Ho Tong Minh's group on Facebook to asking for help when you encounter any problems: [Radar Interferometry community](https://www.facebook.com/share/g/1AEPUi5fdT/)
- Another tutorial for PSInSAR with SNAP and STaMPS from RUS (You can also find a lot of resources from RUS youtube channel and website): [RUS Webinar: SNAP2StaMPS – Data preparation for StaMPS PSI processing with SNAP - HAZA09](https://youtu.be/Xy7Y4Ea5mOo?si=3Kj_JTuYVmk_m2Uk)
### Resouces for setting up SNAP and STaMPS
- To perfrom PSInSAR by SNAP and STaMPS you need MATHLAB, and you are recommended to use Ubuntu or other Linux system. You can do it by installing WMware or VirtualBox in your Windows system.
- To practice with PSInSAR, follow the tutorials from Prof Dinh Ho Tong Minh that I listed above.
- Here are steps to set up your system for PSInSAR processing with SNAP and STaMPS:
    1. Install VMWare (you can download exe file from my Setup folder)
    2. Install Ubuntu: [Ubuntu Download](https://ubuntu.com/download/desktop)
    - remember to allocate enough hard disk space (at least 50 GB) and RAM (at least 8 GB)
    3. Install MATLAB inside Ubuntu: follow instructions in my Setup/MATLAB folder
    - remember to choose 4 required toolboxes: Image Processing Toolbox, Statistics and Machine Learning Toolbox, Parallel Computing Toolbox, Signal Processing Toolbox.
    4. Install SNAP and STaMPS: follow instructions from [1_stamps_setup](https://gitlab.com/Rexthor/gis-blog/-/blob/f5fd5c29d0ce61d17dcb3e671de59b123ed8014d/StaMPS/1_stamps_setup.md?fbclid=IwY2xjawN6eQ9leHRuA2FlbQIxMABicmlkETFaWTRTNE9QdHJ5VTVJSmVGc3J0YwZhcHBfaWQQMjIyMDM5MTc4ODIwMDg5MgABHg0gDvD0NrcK2xc_CTpmna3KrW8xjgbf2jM9Y5lv-2Xlu4xPQAH3RD85HorL_aem_ahn-Xmkr7v0pixFK0yaFmg)