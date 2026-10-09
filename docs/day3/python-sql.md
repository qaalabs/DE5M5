# Activity: Python Meets SQL Server

!!! abstract "S9: Query and manipulate data using tools and programming such as SQL and Python. Manage database access, and implement automated validation checks."

In Module 2 you worked with SQL Server through SSMS. Now Python talks to the same server.

The notebook for this activity lives in a second repo, `library-pipeline-runner`. You will use the same repo again in the next activity.


## 1. Clone the runner repo

Open a new Terminal window, then:

```powershell
cd Desktop
git clone https://github.com/QAADE5/library-pipeline-runner.git
cd library-pipeline-runner
```


## 2. Install dependencies

```powershell
pip install -r requirements.txt
```


## 3. Open SSMS

Open SSMS and connect to `localhost` with **Windows Authentication**.

!!! note "Make sure that you change **Encryption** to `Optional` before you click **Connect**"

!!! info "Keep SSMS open next to the notebook - you will check SSMS after each step."


## 4. Open the notebook

Run Jupyter Notebook in your Virtual Machine.

!!! note "If asked to select an app to open this .html file then select Microsoft Edge and click `Always`"

Find the `library-pipeline-runner/notebooks` folder on the Desktop, then open: `python_sql_server_intro.ipynb`


## 5. Work through the notebook

The notebook has three parts. Work through them at your own pace, running each cell with **Shift + Enter**.

!!! note "At each checkpoint"
    1. Check the result in SSMS
    2. Post the checkpoint number in the chat, e.g. **1 done**
    3. Carry straight on to the next part - do not wait

| Checkpoint | Notebook | You are done when |
|---|---|---|
| **1** | Part 1 - Follow along | SSMS shows 5 rows in `py_sandbox` > `products`, and `products.csv` exists |
| **2** | Part 2 - Make it yours | Your new product and your new column both show in SSMS |
| **3** | Part 3 - On your own | Your own database, table and CSV exist |

Checkpoint 3 is a stretch task. Not everyone will reach it, and that is fine.

!!! success "Leave your Terminal window open - it is already in the right folder for the next two activities."
