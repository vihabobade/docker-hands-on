###########################################
# BASE IMAGE
###########################################

FROM python:3.10-alpine as base
RUN apk add --update --virtual .build-deps \
    build-base \
    # postgresql-dev \
    python3-dev \
    libpq

WORKDIR /app

COPY requirements.txt /app
COPY devops /app

RUN pip install -r requirements.txt && \
    cd devops


############################################
# HERE STARTS THE MAGIC OF MULTI STAGE BUILD
############################################
FROM python:3.10-alpine

# Copy the compiled binary from the build stage
COPY --from=base /app /app


# RUN apk add libpq
COPY --from=base /usr/local/lib/python3.10/site-packages/ /usr/local/lib/python3.10/site-packages/
COPY --from=base /usr/local/bin/ /usr/local/bin/


# Set the entrypoint for the container to run the binary
ENTRYPOINT ["python3"]
CMD ["/app/manage.py", "runserver", "0.0.0.0:8000"]




