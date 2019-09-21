FROM rootproject/root-fedora

# RUN python3 -m pip install --upgrade pip # This causes error, skipping
RUN yum -y update && yum clean all
RUN yum -y install make automake gcc gcc-c++ kernel-devel && yum clean all
RUN python3 -m pip install --ignore-installed tornado==4.4.3
RUN python3 -m pip install jupyter # install jupyter
# RUN python3 -m pip install prompt-toolkit==1.0.15 # deal with prompt-toolkit issue (downgrade)
RUN python3 -m pip install --upgrade ipykernel

# Create the xephyrian user environemnt
RUN useradd -ms /bin/bash xephyrian
# copy xephyr files
WORKDIR /home/xephyrian
RUN git clone https://github.com/XENON1T/Xephyr.git
RUN chown -R xephyrian /home/xephyrian
ENV XEPHYR_DIR /home/xephyrian/
USER xephyrian
RUN mkdir xephyr_projects

# compile xephyr libraries
RUN source Xephyr/pacman/installExample.sh likelihood1D
RUN source Xephyr/pacman/installExample.sh SR1Like
RUN source Xephyr/pacman/build.sh
RUN mv ./build ./xephyr_examples/
RUN root -b "./Xephyr/loadXephyr.C"
RUN root -b "./xephyr_examples/likelihood1D/produceDataAndModels.C"
EXPOSE 8080

# Run jupyter
ENTRYPOINT ["jupyter", "notebook"]
CMD ["--ip=0.0.0.0", "--port=8080", "--allow-root"]
