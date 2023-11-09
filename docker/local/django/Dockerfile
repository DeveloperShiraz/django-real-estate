#First we set the Python version that we want to use.
FROM python:3.10.0-slim-buster

#This is the working directory.
#The home directory is set to "/app"
ENV APP_HOME=/app
#We basically create the directory by using mkdir $APP_HOME
RUN mkdir $APP_HOME
#Creating another directory for staticfiles.
RUN mkdir $APP_HOME/staticfiles
#Then we set the working directory to $APP_HOME.
WORKDIR $APP_HOME

LABEL maintainer='lightningboy78@gmail.com'
LABEL youtube='https://www.youtube.com/@ThatShiraz'
LABEL description="Development image for Real Estate Project"

#We need to prevent Python from creating BYTECODE. The creation of .pyc files...
ENV PYTHONDONTWRITEBYTECODE 1

#We need to set Python output directly to the terminal without being buffered.
#That allows the application output in real-time.
ENV PYTHONUNBUFFERED 1

#The "-y" switch is used to ensure the install goes on without waiting for user prompt.
RUN apt-get update \
  && apt-get install -y build-essential \
  && apt-get install -y libpq-dev \
  && apt-get install -y gettext \
  && apt-get -y install netcat gcc postgresql \
  && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
  && rm -rf /var/lib/apt/lists/*

#This installs and upgrades pip.
RUN pip3 install --upgrade pip
#Now we copy 
COPY ./requirements.txt /app/requirements.txt 

RUN pip3 install -r requirements.txt

COPY ./docker/local/django/entrypoint /entrypoint
RUN sed -i 's/\r$//g' /entrypoint
RUN chmod +x /entrypoint

COPY ./docker/local/django/start /start
RUN sed -i 's/\r$//g' /start
RUN chmod +x /start


COPY ./docker/local/django/celery/worker/start /start-celeryworker
RUN sed -i 's/\r$//g' /start-celeryworker
RUN chmod +x /start-celeryworker

COPY ./docker/local/django/celery/flower/start /start-flower
RUN sed -i 's/\r$//g' /start-flower
RUN chmod +x /start-flower


ENTRYPOINT [ "/entrypoint"]