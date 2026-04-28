找到文件：`/usr/share/polkit-1/actions/org.freedesktop.udisks2.policy`

找到对应的action id， 把相应权限都改成Yes即可.
```xml
<allow_any>yes</allow_any>
<allow_inactive>yes</allow_inactive>
<allow_active>yes</allow_active>
```