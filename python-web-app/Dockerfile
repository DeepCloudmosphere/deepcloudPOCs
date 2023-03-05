
<<<<<<< HEAD
				### Dockerfile ###
=======
				### Dockerfile 1###
>>>>>>> 7fd98aeb12cd3f6bcb0b893f86c549b837ece216

#### Stage 1 ###
# Build virtualenv in apline image 

<<<<<<< HEAD
FROM alpine AS build
=======
FROM alpine AS FLASK
>>>>>>> 7fd98aeb12cd3f6bcb0b893f86c549b837ece216

# Name and email of the person who maintains the file
MAINTAINER Deepak Verma  deepcloud.28@gmail.com

RUN apk add python3 
RUN python3 -m venv /venv &&  /venv/bin/pip install --upgrade pip && source /venv/bin/activate
<<<<<<< HEAD
COPY ./app-source-code/requirements.txt /requirements.txt
RUN /venv/bin/pip3 install --disable-pip-version-check -r /requirements.txt

### Stage 2 ###
#Change file owner  and copy the virtualenv into a distroless image

FROM gcr.io/distroless/python3
COPY --from=build --chown=nonroot:nonroot /venv /venv
COPY ./app-source-code/ /venv 
WORKDIR /venv 
EXPOSE 5000
USER nonroot
ENTRYPOINT ["/venv/bin/python3","-m" , "flask", "run", "--host=0.0.0.0"]


=======
COPY app-source-code/requirements.txt /requirements.txt
RUN /venv/bin/pip3 install --disable-pip-version-check -r /requirements.txt
COPY app-source-code/app.py /venv/
WORKDIR /venv/ 
ENTRYPOINT ["/venv/bin/python3", "app.py"]
>>>>>>> 7fd98aeb12cd3f6bcb0b893f86c549b837ece216
