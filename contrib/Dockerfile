FROM python:3.9-alpine

ARG uid=1000
# Create an unprivileged user:
RUN adduser -u "${uid}" -D django
# Drop privileges:
USER django
# Change the working directory:
WORKDIR /home/django

# Import our wheel from ./dist:
COPY dist/django_tutorial-1.0.0-py3-none-any.whl \
     /tmp/django_tutorial-1.0.0-py3-none-any.whl

# Install the wheel:
RUN pip install                 \
      --no-warn-script-location \
      --no-cache-dir            \
      /tmp/django_tutorial-1.0.0-py3-none-any.whl

# Elevate privileges:
USER root
# Cleanup:
RUN rm /tmp/django_tutorial-1.0.0-py3-none-any.whl
# Drop privileges:
USER django

# Run the migrations (SQLite):
RUN python .local/lib/python3.9/site-packages/django_tutorial/manage.py migrate

SHELL ["/bin/ash", "-o", "pipefail", "-c"]
# WARNING: NEVER DO THIS IN A PRODUCTION DOCKERFILE!
# Create an admin account:
RUN echo "from django.contrib.auth.models import User; User.objects.create_superuser('admin', 'admin@example.com', 'admin')" | python .local/lib/python3.9/site-packages/django_tutorial/manage.py shell

# TODO: Insert some dummy data (seed the DB)

# WARNING: NEVER DO THIS IN A PRODUCTION DOCKERFILE! USE AN ASGI SERVER INSTEAD!
CMD ["python", ".local/lib/python3.9/site-packages/django_tutorial/manage.py", "runserver", "0.0.0.0:8000"]
