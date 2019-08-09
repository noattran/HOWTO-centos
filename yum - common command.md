# 1.	Install from local package
```
$yum localinstall /path/*.rpm_file
```
# 2.	Clean yum cache:
```
$yum clean packages
$yum clean headers
$yum clean metadata
$yum clean all
```
# 3.	Check dependence package
```
$yum whatprovides */package_name
```
