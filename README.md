# Stata Syntax Highlighting for Jupyter Lab 4

Neither [jupyterlab-stata-highlight](https://github.com/kylebarron/jupyterlab-stata-highlight) 
nor [jupyterlab_stata_highlight2](https://github.com/hugetim/jupyterlab_stata_highlight2) works on Jupyter Lab 4.
This is a workaround that needs to be applied everytime Jupyter Lab is updated.

1. Create and activate conda environment.
    ```sh
    conda create -n jupyterlab4 pip nodejs -c conda-forge
    conda activate jupyterlab4
    ```
2. Install Jupyter Lab 4.
    ```sh
    conda install jupyterlab -c conda-forge
    ```
    or
    ```sh
    pip install jupyterlab
    ```
3. Set up the staging files. Make sure you are the owner of the environment, otherwise this will fail.
   ```sh
   jupyter lab build --dev-build
   ```
4. Download Stata syntax highlighting Javascript file.
   ```sh
   wget https://raw.githubusercontent.com/ticoneva/codemirror-legacy-stata/main/stata.js -P $CONDA_PREFIX/share/jupyter/lab/staging/node_modules/@codemirror/legacy-modes/mode/
   ```
5. Modify `$CONDA_PREFIX/share/jupyter/lab/staging/node_modules/@jupyterlab/codemirror/lib/language.js`. Search for `Squirrel` and add the following entry after that one:
```
           {
                name: 'stata',
                displayName: trans.__('Stata'),
                mime: 'text/x-stata',
                extensions: ['do','ado'],
                async load() {
                    const m = await import('@codemirror/legacy-modes/mode/stata');
                    return legacy(m.stata);
                }
            },
```
6. Run `jupyter lab build` again.
