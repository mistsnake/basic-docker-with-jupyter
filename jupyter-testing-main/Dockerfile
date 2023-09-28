FROM ubuntu

# Install updates if necessary
RUN apt update
RUN apt-get install sudo
RUN sudo apt update

# Install python pip
RUN sudo apt install -y python3-pip python3-dev
RUN sudo -H pip3 install --upgrade pip

# Insall jupyter
RUN pip install jupyter

#set the directory for container
RUN mkdir project

# copy all the files to the container
COPY . .

# install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Start jupyter 
CMD jupyter notebook --ip 0.0.0.0 --no-browser --allow-root

