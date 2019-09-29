# Example configurations for Unbound service
[heading__title]:
  #example-configurations-for-unbound-service
  "&#x2B06; Top of ReadMe File"


> Note, this subdirectory may be moved to a separate branch in the future when this repository becomes a full project.



------


#### Table of Contents


- [:arrow_up: Top of ReadMe File][heading__title]

- [:building_construction: Requirements][heading__requirements]

- [:zap: Quick Start][heading__quick_start]

- [:shell: Utilize Unbound][heading__utilize]

- [&#x1F5D2; Notes][notes]

- [:card_index: Attribution][heading__attribution]

- [:balance_scale: License][heading__license]


------



## Requirements
[heading__requirements]:
  #requirements
  "&#x1F3D7; What to install prior to utilizing this project"


```Bash
sudo apt-get install unbound
```


___



## Quick Start
[heading__quick_start]:
  #quick-start
  "&#9889; Perhaps as easy as one, 2.0,..."


Disabled `unbound-resolvconf` service for setup & testing of DNS configurations


```Bash
sudo systemctl disable unbound-resolvconf.service
sudo systemctl stop unbound-resolvconf.service
```


Clone example configurations


```Bash
git clone git@github.com:paranoid-linux/unbound-admin.git
```


Elevate to `root` level permissions for the following, or prepend `sudo` to each command


```Bash
sudo su
```


Backup original configurations


```Bash
mkdir -vp /etc/unbound/originals/unbound.conf.d


mv /etc/unbound/unbound.conf /etc/unbound/originals/
mv /etc/unbound/unbound.conf.d/* /etc/unbound/originals/unbound.conf.d/
```


Copy example configurations


```Bash
cp unbound-admin/example-configs/etc/unbound/unbound.conf /etc/unbound/

cp unbound-admin/example-configs/etc/unbound/unbound.conf.s /etc/unbound/
```


Edit example configurations to suit deployment requirements


```Bash
vim /etc/unbound/unbound.conf.s/00_root-hints.conf
```


Symbolically link any desired configurations from `unbound.conf.s` subdirectory to `unbound.conf.d`


```Bash
ls -s /etc/unbound/unbound.conf.s/00_root-hints.conf /etc/unbound/unbound.conf.d/
## ... etc.
```


___



## Utilize Unbound
[heading__utilize]:
  #utilize-unbound
  "&#x1F41A; How to make use of service on local network"


Restart service


```Bash
sudo systemctl restart unbound.service
```


Check that all is good


```Bash
sudo systemctl status unbound.service
```


> Note, when testing don't restart or stop/start the Unbound service to quickly between tests!



___



## Notes
[notes]:
  #notes
  "&#x1F5D2; Additional notes and links that may be worth clicking in the future"


If utilizing Firejail too, then modify `systemd` configurations...


```Bash
if [ -e "$(which firejail)" ]; then
        sudo mkdir -vp /etc/systemd/system/unbound.service.d

        sudo tee /etc/systemd/system/unbound.service.d/override.conf 1>/dev/null <<EOF
[Service]
ExecStart=
ExecStart=$(which firejail) /usr/local/bin/unbound -d \$DAEMON_OPTS
## Thanks be to: https://github.com/netblue30/firejail/issues/1731
EOF

        ## Reload systemd
        sudo systemctl daemon-reload
fi
```


___



## Attribution
[heading__attribution]:
  #attribution
  "&#x1F4C7; Resources that where helpful in building this project so far."


- [Firejail systemd configurations](https://github.com/netblue30/firejail/issues/1731)


> Note, above is incomplete as this subdirectory is kinda a preview of what'll be published in the future. Feel free to open Pull Requests if current state is bothersome.



___



## License
[heading__license]:
  #license
  "&#x2696; Legal bits of Open Source software"


Legal bits of Open Source software


```
Unbound example configuration quick start
Copyright (C) 2019  S0AndS0

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as published
by the Free Software Foundation; version 3 of the License.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.
```
