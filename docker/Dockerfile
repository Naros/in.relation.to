FROM       fedora:22

# install the required dependencies to complile natice extensions
RUN        dnf -y install gcc-c++ make ruby-devel libxml2-devel libxslt-devel findutils git ruby

RUN        groupadd -r dev && useradd  -g dev -u 1000 dev
RUN        mkdir -p /home/dev
RUN        chown dev:dev /home/dev

# From here we run everything as dev user
USER       dev

# Setup all the env variables needed for ruby
ENV        HOME /home/dev
ENV        GEM_HOME $HOME/.gems
ENV        GEM_PATH $HOME/.gems
ENV        PATH $PATH:$GEM_HOME/bin
ENV        LC_ALL en_US.UTF-8
ENV        LANG en_US.UTF-8
RUN        mkdir $HOME/.gems

# Install Rake and Bundler for driving the Awestruct build & site
RUN        gem install -N rake bundler

# Clone in.relation.to in order to run the setup task
RUN        git clone https://github.com/hibernate/in.relation.to.git $HOME/in.relation.to
RUN        cd $HOME/in.relation.to && git checkout production && rake setup

# We need to patch awestruct to make auto generation work. On mounted volumes file
# change montoring will only work with polling
RUN        gem contents awestruct | grep auto.rb | xargs sed -i "s/^\(.*force_polling =\).*/\1 true/"

EXPOSE     4242
VOLUME     $HOME/in.relation.to
WORKDIR    $HOME/in.relation.to

CMD [ "/bin/bash" ]

