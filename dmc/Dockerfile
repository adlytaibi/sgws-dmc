FROM python:2.7
COPY storagegrid-dmc/ /opt/dmc/storagegrid_dmc/
COPY caroot/ /opt/dmc/storagegrid_dmc/caroot/
WORKDIR /opt/dmc/storagegrid_dmc/
RUN pip install -r installer_scripts/requirement.txt
RUN pip install certifi
RUN cat caroot/root.pem >> /usr/local/lib/python2.7/site-packages/certifi/cacert.pem
CMD ["python", "run.py"]
