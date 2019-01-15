[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/mrakitin/Berlin-2019-bluesky-tutorial/master)

# Bluesky Tutorial

This is a collection of tutorials on data acquisition and analysis with bluesky.
It can be used in an Internet browser with no software installation. Click one
the links below to jump in.

This is free and open to the public. Incidental visitors should
[start here](https://mybinder.org/v2/gh/mrakitin/Berlin-2019-bluesky-tutorial/master).
If you are doing this tutorial as part of a workshop or special event,
[start here](https://a1b555ba89cad11e8a8c4021c84f1ed3-2068931781.us-east-1.elb.amazonaws.com)

**We recommend using Google Chrome for best results, but any modern browser
is supported.**

## Survey
Took our tutorial? Let us know how you thought of it so we can better improve
it!
[Survey](https://goo.gl/forms/WAWhkAIvEGVzIUdf2)

## References

* [NSLS-II Software Documentation Landing Page](https://nsls-ii.github.io)
* [Bluesky Documentation](https://nsls-ii.github.io/bluesky)
* [Ophyd Documentation](https://nsls-ii.github.io/ophyd)
* [Databroker Documentation](https://nsls-ii.github.io/databroker)
* [Our Gitter Chat Channel](https://gitter.im/NSLS-II/DAMA) (come here for questions)
* [Python Help](https://www.oreilly.com/programming/free/files/python-for-scientists.pdf) : A collection of Python tutorials geared towards scientific data analysis.


## Contributing to this Tutorial

### Making Changes

Install the software requirements.

```
pip install -r requirements.txt
```

Set EPICS-related environment variables, necessary for some examples involving
pyepics or caproto.

```
source .env
```

Start Jupyter and edit away!

```
jupyter notebook
```

### Publishing Updates

#### Binder deployment

The Binder deployment will update automatically the first time someone requests
a session.

#### BNL JupyterHub deployment on AWS

The BNL JupyterHub deployment will automatically pull fresh copies of the
*content* but if the software requirements change, the Docker image must be
manually updated.

To build an publish an updated version of the Docker image:

```
jupyter-repo2docker --user-name=jovyan --no-run --image-name nsls2/tutorial:$(git rev-parse --short=6 HEAD) .
docker push $(git rev-parse --short=6 HEAD)
```

Using the first six characters of the current commit hash as the Docker tag is
just a convention to help us stay organized. If re-building a new copy of the
same content but with updated dependencies (say, pulling in an updated version
of bluesky), add a ``.N`` counting number after the hash, starting at ``.1``.

Then in the CI host on AWS, update the tag in the JupyterHub helm configuration,
``config.yaml``, and redeploy:

```
helm list  # Find <DEPLOYMENT_NAME>.
helm upgrade <DEPLOYMENT_NAME> jupyterhub/jupyterhub --version=v0.6 -f config.yaml
```
