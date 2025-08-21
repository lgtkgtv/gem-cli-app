```
cd ~/gem-cli-app/
uv python install 3.12
uv venv
uv init
uv pip install jupyterlab ipython ipykernel numpy pandas scikit-learn matplotlib torch  


VIRTUAL_ENV=$(pwd)/.venv uv run ipython kernel install \
  --user \
  --name=my_notebook \
  --display-name "my_notebook"

VIRTUAL_ENV=$(pwd)/.venv uv run ipython kernel install   --user   --name=my_notebook   --display-name "my_notebook"

    OR

# source .venv/bin/activate
# uv run ipython kernel install --user --name=my_notebook --display-name "my_notebook"

jupyter-lab
```
