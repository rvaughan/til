# SSH Client

I needed to be able to upload data to a remote Linux machine from within an existing Golang utility.

Using [this](https://blog.tarkalabs.com/ssh-recipes-in-go-part-one-5f5a44417282) article I put together the following quick piece of code:

```golang
package main

import (
	"fmt"
	"io"
	"io/ioutil"
	"os"

	"golang.org/x/crypto/ssh"
)

func publicKey(path string) ssh.AuthMethod {
	key, err := ioutil.ReadFile(path)
	if err != nil {
	 	panic(err)
	}

	signer, err := ssh.ParsePrivateKey(key)
	if err != nil {
	 	panic(err)
	}

	return ssh.PublicKeys(signer)
}

func runCommand(conn *ssh.Client, cmd string) {
	sess, err := conn.NewSession()
	if err != nil {
	 	panic(err)
	}
	defer sess.Close()

	sessStdOut, err := sess.StdoutPipe()
	if err != nil {
	 	panic(err)
	}
	
	go io.Copy(os.Stdout, sessStdOut)
	
	sessStderr, err := sess.StderrPipe()
	if err != nil {
	 	panic(err)
	}
	
	go io.Copy(os.Stderr, sessStderr)

   	err = sess.Run(cmd)
	if err != nil {
	 	panic(err)
	}
}

func main() {
	fmt.Println("go_ssh starting.")

	config := &ssh.ClientConfig {
		User: "username",
		Auth: []ssh.AuthMethod{ 
			publicKey("test_key"),
		},
		HostKeyCallback: ssh.InsecureIgnoreHostKey(),
	}

	conn, err := ssh.Dial("tcp", "localhost:22", config)
	if err != nil {
		panic(err)
	}
	defer conn.Close()

	// Check who we are logged in as
	runCommand(conn, "/usr/bin/whoami")

	// open an SFTP session over an existing ssh connection.
	sftp, err := sftp.NewClient(conn)
	if err != nil {
		panic(err)
	}
	defer sftp.Close()

	// Open the source file
	srcFile, err := os.Open("test_file")
	if err != nil {
		panic(err)
	}
	defer srcFile.Close()

	// Create the destination file
	dstFile, err := sftp.Create("dest_file")
	if err != nil {
		panic(err)
	}
	defer dstFile.Close()

	// write to file
	if  _, err := dstFile.ReadFrom(srcFile); err != nil {
		panic(err)
	}
}
```
