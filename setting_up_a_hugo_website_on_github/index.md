# Setting_up_a_hugo_website_on_github


# Steps

1. Find out which static site generator suits me the best
    * Jekyll - Ruby
    * Hexo - Node.js
    * Hugo - Go
    Hugo is the fastest, and it has less dependency issues.

2. Install Go
    * To install golang-1.18.1 on Ubuntu 22.04 jammy
    ```shell
        apt install golang
    ```
    * To install newer golang on Ubuntu 22.04 jammy
    ```shell
        sudo add-apt-repository ppa:longsleep/golang-backports
        sudo apt update
        sudo apt install golang-1.21.7
    ```
    * Add Golang Versions to Alternatives system
    ```shell
        sudo update-alternatives --install /usr/bin/go go /usr/lib/go-1.18/bin/go 118
        sudo update-alternatives --install /usr/bin/go go /usr/lib/go-1.21/bin/go 121
    ```
    * List available Golang Alternatives
    ```shell
        sudo update-alternatives --list go
    ```
    * Switch between Golang Versions
    ```shell
        sudo update-alternatives --configure go
    ```

3. Install Hugo
    ```shell
        apt install hugo
    ```

4. Create new site
    ```shell
        hugo new site my-hugo-blog
    ```

5. Install LoveIt theme, check out from github's LoveIt repository
    ```shell
        git init
        git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt`
    ```
6. Configure hugo, copy from example site and edit
    ```shell
        cp themes/LoveIt/exampleSite/config.toml ./
    ```
   Change themesDir to "../.." to "themes", this is a bug in the example
   Configure other parameters of your site if needed
   
7. Create the first post and Edit
    ```shell
        hugo new posts/first.md
    ```
    And then edit the first post at content/posts/helloworld.md
    ```shell
        $ cat content/posts/helloworld.md 
        ---
        title: "Helloworld"
        date: 2024-02-11T18:49:18+08:00
        draft: true
        ---
    ```

8. Preview in a browser
    ```shell
        hugo server -D
    ```
    Add -D to show all drafts in preview, or change draft attribute to false in the posts' md files

    If all goes well, you should be able to see your site at http://localhost:1313/
    I edit md file in VS Code via Remote SSH, And "forwarded ports" is a very nice feature, it allows the web browser to connect to the remote ssh node's port 1313 to preview the contents.

9. Build
    
    Build your site with command 
    ```shell
        hugo
    ```
    The command buils all the site's contents into public folder.

10. Deploy
    * Create a repository like "etheroz.github.io" on Github. The name of the repository **Make sure it is a public repository. you cannot publish a private repository to Github Pages.**  
    * Push the contents to GitHub
    ```shell
        cd public
        # GitHub's default branch is main, git's default branch is master, you can neither change the default branch on GitHub or in the local git config, or use "-b main"
        git init -b main
        git add .
        git commit -m "My first commit"
        git remote add origin git@github.com:etheroz/etheroz.github.io.git
        git pull origin main # fetch and merge existing README or other files
        git push origin main
    ```
    * Go to your repository's settings page, click Pages on the left panel to navigate to GitHub Pages configuration, choose "Deploy from a branch", choose "main" branch's root(/) to deploy.

11. Add new post
    
    When adding a new post, you have to run hugo again to regenerate the whole site again, so that is why speed is very important when there are hundreds of posts to be generated, and Hugo is the fastest so far. The steps are like:
    ```shell
        hugo # rebuild the static site, including your latest post
        cd public
        git add .
        git commit -m "posting a blog"
        git push origin main # deploy!
    ```
# Notes

1. Added RSA SSH Key into github, but ED25519 is the default on github and Ubuntu 22.04
    Switch to ED25519 
    ```shell
        # Generat the key
        ssh-keygen -t ed25519 -C "you@email.com"
        # Launch ssh-agent
        eval $(ssh-agent)
        # Add the private key to ssh-agent
        ssh-add ~/.ssh/id_ed25519
        # List the public key and use the public key to add a ssh connection on Github
        cat ~/.ssh/id_ed25519.pub
        # Test ssh connection to Github
        ssh -T git@github.com
        "Hi etheroz! You've successfully authenticated, but GitHub does not provide shell access."
    ```

2. Verify your ssh key's fingerprint
    ```shell
        ssh-keygen -lf ~/.ssh/id_ed25519
    ```
   prints the fingerprint of your private key, after importing your public key to create a SSH connection with your GitHub account, you'd better check if fingerprint matches 

3. Your repository must be a public repository, otherwise you cannot publish your repository to GitHub Pages.  Go to the repository's public settings, and change the repository's visibility to public.


### References:
* https://zhuanlan.zhihu.com/p/368407566
* https://entra.ca/blog/build-blog/
* https://cuttontail.blog/blog/create-a-wesite-using-github-pages-and-hugo/
* https://jdhao.github.io/2018/10/10/hexo_to_hugo/
* https://gohugo.io/getting-started/quick-start/
* https://hugoloveit.com/zh-cn/theme-documentation-basics/




