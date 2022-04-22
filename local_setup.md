# Local setup

## Install brew
```commandline
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
append eval "$(/opt/homebrew/bin/brew shellenv)" to ~/.zprofile
source ~/.zprofile

append alias brew='env PATH="${PATH//$(pyenv root)\/shims:/}" brew' to ~/.zshrc
echo "export LDFLAGS=\"\-L\${HOMEBREW_PREFIX}/lib\"" >> ~/.zshrc
echo "export CPPFLAGS=\"\-I\${HOMEBREW_PREFIX}/include\"" >> ~/.zshrc
source ~/.zshrc
```

## Install pyenv and pyenv-virtualenv
```commandLine
brew install pyenv
brew install pyenv-virtualenv
append eval "$(pyenv init --path)"' to ~/.zprofile
append eval "$(pyenv init -)" to ~/.zshrc
append eval "$(pyenv virtualenv-init -)" to ~/.zshrc

source ~/.zprofile
source ~/.zshrc
```

## Configure pip

~/.netrc
```
machine artifactory.build.upgrade.com
    login readonlyartifactory
    password AP8TRS38seGXAFRmfwPqGLTVYdyUJT2tF7Vp4u
```

```sh
mkdir -p ~/.config/pip/
```

~/.config/pip/pip.conf

```
[global]
extra-index-url = https://artifactory.build.upgrade.com/artifactory/api/pypi/pypi/simple
                  https://artifactory.build.upgrade.com/artifactory/api/pypi/pypi-snapshots/simple
```

## Configure maven
Download settings.xml from https://github.com/Credify/devops/blob/master/settings.xml to ~/.m2 directory.

## Configure Docker
```sh
brew install docker
```

~/.docker/config.json
```json
{
    "auths": {
        "docker-release.artifactory.build.upgrade.com": {
            "auth": "ZG9ja2VyLXJvOndDZGZXa2l1aW9wTkkzbTE1OQ=="
        },
        "dockerhub.artifactory.build.upgrade.com": {
            "auth": "ZG9ja2VyLXJvOndDZGZXa2l1aW9wTkkzbTE1OQ=="
        },
        "docker-upgrade.artifactory.build.upgrade.com": {
            "auth": "ZG9ja2VyLXJvOndDZGZXa2l1aW9wTkkzbTE1OQ=="
        }
    }
}
```

## Instal Amazon Corretto JDK 11
Use Self Service

export JAVA_HOME=/Library/Java/JavaVirtualMachines/amazon-corretto-11.jdk/Contents/Home

## Checkout upflow
```sh
pyenv install 3.8.10
pyenv virtualenv 3.8.10 upflow

git clone git@github.com:Credify/upflow-base.git
git clone git@github.com:Credify/upflow.git

echo upflow > upflow-base/.python-version
echo upflow > upflow/.python-version

cd upflow
git clone git@github.com:Credify/airflow-extras.git
ln -s airflow-extras/airflow_extras .

brew install freetds
brew install snappy
brew install openssl@1.1
brew link openssl@1.1 --force
echo "export GRPC_PYTHON_BUILD_SYSTEM_OPENSSL=1" >> ~/.zshrc

pip install -r ../upflow-base/requirements.txt
pip install -r airflow-extras/localrequirements.txt

pytest airflow-extras/tests/
pytest tests/
```

## Checkout tubo
```sh
pyenv install 3.9.8
pyenv virtualenv 3.9.8 tubo

git clone git@github.com:Credify/tubo.git
cd tubo
echo tubo > .python-version

brew install unixodbc
pip install -r requirements.txt
pip install pyyaml==6.0.0
pip install mock==4.0.3
pip install docker==5.0.3

TUBO_DIR=$(pwd) TUBO_ENV=test pytest
```

## Other apps
```sh
brew install --cask iterm2
brew install --cask keybase
brew install --cask visual-studio-code
brew install --cask pycharm-ce
brew install --cask intellij-idea-ce
brew install --cask dbvisualizer
brew install --cask 1password
```
