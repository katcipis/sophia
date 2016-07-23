
How to handle when you acquire two resources that must be manually
released ? In go it would be trivial using defer, in exception
oriented languages, well, not that good.

tempdir = tempfile.mkdtemp()
try:
    driver = _new_firefox_driver(_get_firefox_profile(tempdir))
    try:
        return _download_csv(driver, tempdir, row)
    finally:
        driver.close()
finally:
    shutil.rmtree(tempdir)
