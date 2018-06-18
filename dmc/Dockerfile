FROM python:2.7
COPY storagegrid-dmc/ /opt/dmc/storagegrid_dmc/
WORKDIR /opt/dmc/storagegrid_dmc/
RUN pip install -r installer_scripts/requirement.txt
CMD ["python", "run.py"]
