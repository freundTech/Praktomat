FROM debian:jessie

MAINTAINER Martin Hecker <martin.hecker@kit.edu>

RUN echo "deb http://http.debian.net/debian jessie-backports main" > /etc/apt/sources.list.d/jessie-backports.list
RUN apt-get --yes update
RUN apt-get update && apt-get install -y --no-install-recommends apt-utils
RUN apt-get install --yes -t jessie-backports ca-certificates-java
RUN apt-get install --yes openjdk-8-jdk junit junit4 dejagnu gcj-jdk && apt-get clean
RUN update-java-alternatives -s java-1.8.0-openjdk

# Python-Stuff
RUN apt-get update -y
RUN apt-get install --yes python3            python               && apt-get clean
RUN apt-get install --yes                    ipython              && apt-get clean
RUN apt-get install --yes python3-requests                        && apt-get clean
RUN apt-get install --yes python3-pip                             && apt-get clean
RUN apt-get install --yes python3-six        python-six           && apt-get clean
RUN apt-get install --yes python3-responses                       && apt-get clean
RUN apt-get install --yes python3-xlrd                            && apt-get clean
RUN apt-get install --yes python3-simplejson python-simplejson    && apt-get clean


RUN python3 -m pip install --upgrade pip
RUN pip3 install numpy scipy matplotlib ipython pandas
RUN sed -i -e 's/^backend      : \(\(TkAgg\)\|\(tkagg\)\)$/backend      : Agg/g' /usr/local/lib/python3.4/dist-packages/matplotlib/mpl-data/matplotlibrc

# set up some font cache once and for all
RUN python3 -c "from matplotlib.font_manager import FontManager"

RUN pip3 install statsmodels
RUN pip3 install scikit-learn
RUN pip3 install numdifftools
RUN pip3 install arch

# Install Isabelle2016
RUN apt-get install --yes -t jessie-backports curl                && apt-get clean
RUN curl http://isabelle.in.tum.de/dist/Isabelle2017_app.tar.gz | tar -C /opt -xz
RUN ln -s /opt/Isabelle2017/bin/isabelle /usr/local/bin
RUN isabelle build -sbv HOL



###################SCALA###################
RUN apt-get install --yes wget && apt-get clean

ENV SCALA_VERSION 2.11.7
ENV SBT_VERSION 0.13.12

ENV SBT_OPTS -Xmx2G -XX:+UseConcMarkSweepGC -XX:+CMSClassUnloadingEnabled -Xss2M -Duser.timezone=GMT

# install sbt
RUN wget https://dl.bintray.com/sbt/debian/sbt-$SBT_VERSION.deb
RUN dpkg -i sbt-$SBT_VERSION.deb

# install scala
RUN wget https://downloads.typesafe.com/scala/$SCALA_VERSION/scala-$SCALA_VERSION.deb
RUN dpkg -i scala-$SCALA_VERSION.deb

RUN sbt

#########################################


###################GHC###################

RUN apt-get install --yes ghc libghc-test-framework-dev libghc-test-framework-hunit-dev libghc-test-framework-quickcheck2-dev

#########################################


RUN echo "praktomat:x:1001:1001:,,,:/home/praktomat:/bin/bash" >> /etc/passwd
RUN echo "praktomat:x:1001:tester" >> /etc/group

# We use a fresh tmpfs with /home in each container
RUN chmod 1777 /home
