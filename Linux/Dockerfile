FROM debian:stable-slim

RUN echo "*** Installing gcc and clang ***" \
  && DEBIAN_FRONTEND=noninteractive apt-get update \
  && apt-get dist-upgrade -y \
  && echo "deb http://ftp.us.debian.org/debian jessie main contrib non-free" >> /etc/apt/sources.list.d/jessie.list \
  && echo "deb http://ftp.us.debian.org/debian unstable main contrib non-free" >> /etc/apt/sources.list.d/unstable.list \
  && apt-get update
RUN apt install python -y
RUN apt install python3 -y
RUN apt-get install -y cmake \
  && apt-get install -y git \
  && apt-get install wget
RUN apt-get install -y g++-10
RUN apt-get install -y clang++-10
RUN apt-get install libc++-dev -y
RUN apt-get install libc++abi-dev -y \
&& apt-get autoremove --purge -y
RUN apt-get autoclean -y
RUN rm -rf /var/cache/apt/* /tmp/*
RUN echo "Setting g++ 10.1 as default compiler"
RUN update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-10 1
RUN echo "Setting python3.8 as default" \
  && update-alternatives --install /usr/bin/python python /usr/bin/python3.8 1 \
  && update-alternatives --install /usr/bin/python python /usr/bin/python2.7 2
RUN echo "Getting polly..." \
  && wget --no-check-certificate -O v0.10.3.tar.gz https://github.com/ruslo/polly/archive/v0.10.3.tar.gz \
  && tar xvzf v0.10.3.tar.gz \
  && echo "Setting environment variables" \
  && echo "... Adding /usr/bin/clang++-6.0/bin to PATH variable" \
  && export PATH=/usr/bin/clang++-6.0/bin:$PATH \
  && echo "... Adding /usr/lib/clang/6.0/lib to LD_LIBRARY_PATH variable" \
  && export LD_LIBRARY_PATH=/usr/lib/clang/6.0/lib:$LD_LIBRARY_PATH \
  && ln -s /usr/bin/clang++-6.0 /usr/bin/clang++ \ 
  && ln -s /usr/bin/clang-6.0 /usr/bin/clang \
  && echo "... Adding /polly-0.10.3 to POLLY_ROOT variable" \
  && export POLLY_ROOT=/polly-0.10.3


#  RUN echo "... Getting opengl libraries" \
#  && echo "test"  \
#  && wget --no-check-certificate -O opengl_new.tar.gz https://github.com/kaizenman/install_opengl/archive/opengl_new.tar.gz \
#  && tar xvzf opengl_new.tar.gz

  RUN echo "Installing RANDR!!!"
  RUN yes | apt-get install xorg-dev libglu1-mesa-dev libgl1-mesa-dev freeglut3-dev
  RUN git clone https://github.com/glfw/glfw \
    && cd glfw \
    && mkdir build \
    && cd build \
    && cmake .. && make -j4 \
    && make install

  WORKDIR /

  RUN mkdir ./include

  RUN cp -Rv ./glfw/include/ ./include/ 
  RUN rm -r glfw

# NVIDIA driver
  #RUN  echo "deb http://httpredir.debian.org/debian stretch-backports main contrib non-free" >> /etc/apt/sources.list \
  #&& apt-get install -t stretch-backports nvidia-driver -y

  #RUN apt-get install libacl1-dev -y \
  #&& apt-get install libcap-dev

# BOOST library
RUN cd /home && wget http://downloads.sourceforge.net/project/boost/boost/1.73.0/boost_1_73_0.tar.gz \
  && tar xfz boost_1_73_0.tar.gz \
  && rm boost_1_73_0.tar.gz \
  && cd boost_1_73_0 \
  && ./bootstrap.sh --prefix=/usr/local --with-libraries=program_options \
  && ./b2 install \
  && cd /home \
  && rm -rf boost_1_73_0

CMD [ "/bin/bash" ]