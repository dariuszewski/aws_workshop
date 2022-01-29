# LAB1 Configuration of cloud enviroment

## 0. Useful resources

1. dinopass.com -> random password generator
2. VSCode extension: `Remote - SSH`
<hr>

## 1. Logging into Sokrates

1. add `ssh students_id@sokrates.edu.jkan.pl`
2. password: `uekpiekn`
<hr>

## 2. SSH authorization
<br>

#### 1. ssh for github


1. generate key -> `ssh-keygen`
2. generated key can be named and secured with passphrase (123qwe)

>this will create two versions of keys `id_rsa` - private (longer), and `id_rsa.pub` - public (shorter)

3. add new ssh key in git -> paste public key
4. connect to git via ssh: 
<br>
`ssh git@github.com -i ~/.ssh/id_rsa`

5. Github doesn't provide shell access anymore ; - )
<br>

<hr>

## 2. Amazon's EC2 Virtual Machine

### 1. Change region to `Frankfurt`
### 2. Create and instance of VM: 
1. AMI : `Amazon Linux 2 - Kernel 5.1`
2. Type: `t2.micro`
3. Details: `Enable Public IP`
4. Storage: `default`
5. Add TAGs: `add Name key for VM`
6. Security group: `Select existing security group -> students`
7. Launch: `Select a key pair -> StudentKey | RSA`

### 3. Login to VM

1. Look up `Public IPv4 address` of your instance
2. connect using:
`ssh ec2-user@IP_ADDR` (this will fail now)
3. Get the private key from `_static/id_student` (`s**1`)
4. download key with: `wget path/to/key`
5. Key is unprotected, change it to `chmod 400 id_student`
6. now login with `ssh ec2-user@IP_ADDR -i ./id_student` and passphrase

<hr>

### 4. Save key on VM

```
$ eval `ssh-agent`
$ ssh-add ./id_student
$ ssh ec2-user@IP_ADDR whoami ... etc : )
 ```
