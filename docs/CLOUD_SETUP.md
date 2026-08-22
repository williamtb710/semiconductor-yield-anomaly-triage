# Cloud Setup: Google Colab + GitHub

This workflow is intentionally beginner-friendly and uses no secrets. The notebook downloads the public UCI files directly, so do **not** add a GitHub token, Google credential, API key, or Drive mount.

## A. Create the GitHub repository

1. Download and unzip the starter bundle from this task.
2. Sign in at [github.com](https://github.com/).
3. In the upper-right corner, click the **+** menu, then **New repository**.
4. Set **Repository name** to `semiconductor-yield-anomaly-triage`.
5. Use this description: `Leakage-aware rare-failure triage case study using the public UCI SECOM dataset.`
6. Choose **Public** for the simplest Colab access and a conference-shareable portfolio. Choose **Private** only if you want to hide work in progress; Colab may then ask you to authorize access.
7. Do **not** add a README, `.gitignore`, or license—the bundle already contains the first two.
8. Click **Create repository**.
9. On the empty repository page, click **uploading an existing file**. If the repository is not empty, use **Add file → Upload files**.
10. Open the unzipped starter folder and drag **its contents** into the upload area. Confirm that GitHub shows `README.md`, `docs/`, and `notebooks/` rather than one extra enclosing folder.
11. Enter commit message `Add Checkpoint 1 project scaffold` and click **Commit changes**.

GitHub’s current official instructions use the same **+ → New repository** and **Add file → Upload files** flow: [create a repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository) and [upload files](https://docs.github.com/en/repositories/working-with-files/managing-files/adding-a-file-to-a-repository).

## B. Open and run the notebook in Colab

1. In GitHub, open `notebooks/01_data_audit.ipynb`.
2. Copy the page URL. It will look like:
   `https://github.com/YOUR_USERNAME/semiconductor-yield-anomaly-triage/blob/main/notebooks/01_data_audit.ipynb`
3. Go to [Google Colab](https://colab.research.google.com/).
4. Choose **File → Open notebook → GitHub**.
5. Paste either the repository URL or your GitHub username, then select `01_data_audit.ipynb`.
6. In Colab, choose **Runtime → Change runtime type** and set **Hardware accelerator** to **None**. CPU is more than enough.
7. Choose **Runtime → Run all**.
8. If Colab warns that the notebook was not authored by Google, inspect the source and click **Run anyway**. This notebook downloads only from the official UCI URL and writes only under the temporary runtime directory.
9. Wait for the final green `CHECKPOINT 1 COMPLETE` message.
10. In the left sidebar, click the **Files** folder icon. You should see `reports/` and `checkpoint_1_outputs.zip`. Right-click the ZIP and choose **Download** if you want a local copy of the generated report.

Colab’s official FAQ confirms that notebooks can be loaded from GitHub and that the runtime VM and installed files are temporary, which is why the notebook contains its own dependency and download steps: [Google Colab FAQ](https://research.google.com/colaboratory/faq.html).

## C. Save notebook results back to GitHub

For Checkpoint 1, first send the requested output values back in this task. After we review them:

1. In Colab choose **File → Save a copy in GitHub**.
2. Authorize GitHub if prompted.
3. Select your repository, branch `main`, and path `notebooks/01_data_audit.ipynb`.
4. Use commit message `Record Checkpoint 1 audit outputs`.
5. Leave **Include a link to Colaboratory** checked if shown, then save.

Saving the notebook may include code-cell outputs. That is acceptable here because the output is public dataset summary information and contains no secrets. Never paste tokens into notebook cells. GitHub explicitly advises against committing credentials: [storing secrets safely](https://docs.github.com/en/get-started/learning-to-code/storing-your-secrets-safely).

## D. Multi-computer habit

- Start each session from the notebook in GitHub, not from an old browser tab.
- Save a copy back to GitHub after a meaningful checkpoint with a clear commit message.
- Treat the Colab VM as disposable: anything only under `/content` can disappear when the runtime ends.
- Generated raw data stay out of Git. Small final reports/figures can be uploaded after we review them.

## Fallbacks

### GitHub unavailable, Colab available

1. Open Colab.
2. Choose **File → Upload notebook**.
3. Upload `notebooks/01_data_audit.ipynb` from the downloaded bundle.
4. Run all cells and download `checkpoint_1_outputs.zip` from the Files sidebar.
5. Save a copy to Google Drive until GitHub is available again.

### Colab unavailable

Run locally with Python 3.12 and Jupyter using the commands in the root README. The notebook is standalone and downloads the same official archive.

### Both services unavailable

Keep the downloaded starter bundle. Run the notebook later in any Jupyter-compatible cloud service or local environment. If execution itself is blocked, upload the notebook or any error text in this task and continue the checkpoint here; do not substitute a mirror dataset without documenting it.
