# Backshop: Ansible 101

## Installing Ansible and tools for demo

```
brew update
brew install caskroom/cask/brew-cask
sudo brew cask install virtualbox
sudo brew cask install vagrant
brew install ansible
vagrant box add --provider virtualbox boxcutter/ubuntu1410
```

## Provisioning hosts on Vagrant

```
git clone https://github.com/mtpereira/backshop-ansible-101
cd backshop-ansible-101/demo/
vagrant up
```

## Running demos

Example for demo 1:

```
cd 1/
ansible-playbook -i ../inventory playbook.yml
```

## Cleanup

```
vagrant destroy
vagrant box remove boxcutter/ubuntu1410
```

