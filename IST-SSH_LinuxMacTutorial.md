[source](https://www.youtube.com/watch?v=vpk_1gldOAE)

# Creating SSH keys manually
1. Create SSH Keys
   - private: on machine that will be doing the remoting
   - public: machine you'll log into

```bash
ssh-keygen -t rsa -b 4096
```

1. Keep default location after asking where to save
2. Leave passphrase blank unless you're worried about someone accessing your local computer
3. Go to directory containing keys

```bash
cd ~/.ssh

# view new files created
ls -la
```
5. `id_rsa` is private key, `id_rsa.pub` is public key
6. Copy the public key to remote machine that you'll log into (macbook pro)
7. First make sure the remote machine has a `.ssh` directory
8. If does not, make it

```bash
mkdir .ssh
```
9. copy public key to remote machine

```bash
scp ~/.ssh/id_rsa.pub oliveroliverio@<ipAddress>:/Users/oliveroliverio/.ssh/uploaded_key.pub

# see if uploaded
ls .ssh/
```
10. Append `uploaded_key.pub` to `authorized_keys` file (automatically created after running the following)

```bash
cat ~/.ssh/uploaded_key.pub >> ~/.ssh/authorized_keys

# view the file
cat ~/.ssh/authorized_keys
```
11. setup permissions
12. SSH folder set to `700`, files within this folder set to `600`

```bash
chmod 700 ~/.ssh/
chmod 600 ~/.ssh/*
```

13. Now you should be able to SSH into machine without password
14. Maximize security by only allowing this kind of login and disabling password logging by editing SSH config file
15. First back up original `sshd_config` file

```bash
# back up original
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak

# open original and turn off pw authentication
vi /etc/ssh/sshd_config
```

16. look for `#PassowrdAuthentication` and change to `no` and uncomment by removing the `#`
17. restart SSH service on host computer and

```bash
sudo service ssh restart
```
----------------------------------------------------------------

# Creating SSH keys: the easier way

1. Generate keys like previously

```bash
ssh-keygen -t rsa -b 4096
```
2. Install `ssh-copy-id`

```bash
brew install ssh-copy-id
```
3. run it

```bash
ssh-copy-id oliveroliverio@<ipAddress>

# type in your pw and press enter
```

4.  Done, go see some stuff

```
ls -la

# look for .ssh folder

```
5. Be sure to change the `sshd_config` file