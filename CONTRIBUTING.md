# DC/OS documentation contribution guidelines

## Styling and formatting your contribution

- Use [GitHub-flavored markdown](https://help.github.com/enterprise/11.10.340/user/articles/github-flavored-markdown/).

- Use relative links.

- Begin all links at the root `docs` level and include the version number subdirectory. (e.g., `/docs/1.8/administration/sshcluster/`).

- Each directory must contain an `index.md` file. This acts as the base-level topic for each folder in the site (required).

- Do not include file names in your paths. Our site converts any files not named `index.md` into directory names. For example, the directory `/docs/1.8/administration/` contains a file named `user-management.md`. To link to this content on the live site, you would use the following path: `/docs/1.8/administration/user-management/`.

- The table of contents of each page is automatically generated based on the top-level headers.

- Directory tables of contents are automatically generated based on `post_title` (or `nav_title`) and `post_excerpt` headers.

- Use active voice whenever possible.

- Use sentence-style capitalization for headings.

## Making your contribution

### Modifying the content

1. Create a [JIRA issue](https://dcosjira.atlassian.net/secure/CreateIssue!default.jspa) and select **docs** as the component.

1. [Fork](https://help.github.com/articles/fork-a-repo/) the [dcos-docs](https://github.com/dcos/dcos-docs) repo (you only have to do this once).

1. Clone your fork of the [dcos-docs](https://github.com/dcos/dcos-docs) repo.

    ```bash
    $ git clone https://github.com/<your-user-name>/dcos-docs
    ```

1. Create a branch on your fork using your JIRA number as the name.

    ```bash
    $ git checkout -b dcos-nnn
    ```

1. Create your content. In most cases you should be able to create your content within the existing directory structure. 

    - To create a single page:
        1. Create a markdown file `{post_slug}.md` where `post_slug` is your file name. File names become URIs. If you want this page to be a child of another page, place the `.md` file in the parent folder.
        1. Add your page content, including the required metadata `post_title` and optional `nav_title` and `menu_order`. Do not include any other metadata.
        
               ```
               ---
               post_title: The Title
               ---
               Post markdown goes here.
               ```
    - To create a page with hierarchy:
        1. Create a new directory in the appropriate location of the correctly versioned release (e.g., `/1.8/foo`) and a child page within this folder named `index.md` (e.g. `/1.8/foo/index.md`). The actual URI of your page will be `/1.8/foo/`, not `/1.8/foo/index`. For example, if it's a tutorial for 1.7, create a new directory here `/1.8/usage/tutorials/foo/`.
        1. Add your page content, including the required metadata `post_title` and optional `nav_title` and `menu_order`. Do not include any other metadata.
                
               ```bash
               ---
               post_title: The Title
               ---
               Post markdown goes here.
               ```

    - **Tips:** 
        * Check the `templates` directory to see if there is a template that corresponds to the type of content you are creating. For example, if it's a tutorial you can copy [templates/tutorial.md](templates/tutorial.md) into `foo/` and rename it to `foo/index.md`. Adapt the sections in your new `foo/index.md` to the specifics of your content.

        * Add images in a child `foo/img/` directory.  

        * Include all required assets in your `/foo` directory, for example, Marathon app spec, JSON docs, or a Dockerfile.

        * If you're unsure about what exactly should go into a tutorial, you can always check out [spark/](/docs/1.8/usage/tutorials/spark/) for reference.

1. Push your changes into the feature branch of your remote.

    ```bash
    $ git add .
    $ git commit -m "Addresses issue DCOS-nnn"
    $ git push origin dcos-nnn
    ```

### <a name="test-local"></a>Building and testing your content locally

We've implemented the [dcos-docs](https://github.com/dcos/dcos-docs) repo as a [submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules) of the [dcos-website](https://github.com/dcos/dcos-website) repo. Before submitting your pull request against the [dcos-docs](https://github.com/dcos/dcos-docs) repo, fork the parent [dcos-website](https://github.com/dcos/dcos-website) repo and build the site locally. This will allow you to confirm that your content renders correctly and that all of your links work. 

#### Prerequisites:

1.  Install the prerequisites:

    1.  [Ruby](https://www.ruby-lang.org/en/documentation/installation/) 
        
        *CentOS*
        
        ```bash
        $ sudo yum install -y ruby
        ```
        
    1.  [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
    
        *CentOS*
                
        ```bash
        $ sudo yum install git
        ```
        
    1.  Install EPEL repo, [Node](https://docs.npmjs.com/getting-started/installing-node), and NPM.
    
        *CentOS*
        
        ```bash
        $ sudo yum install -y epel-release && sudo yum install -y nodejs && sudo yum install -y npm && npm install && npm update
        ```
        
    1.  nvm 6.3.1
        
        *CentOS*
        
        ```bash
        $ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.4/install.sh | bash
        $ nvm install 6.3.1 && nvm alias default 6.3.1
        ```
        
    1.  [Gulp](http://gulpjs.com/)
    
        *CentOS*
        
        ```bash
        $ sudo npm install --global gulp-cli
        ```

1. Clone the dcos-website repo.

    ```bash
    $ git clone https://github.com/dcos/dcos-website 
    ```

1. Check out the develop branch of `dcos-website`.

    ```bash
    $ git checkout develop
    ```

1. Initialize the `dcos-docs` submodule with the content from the upstream master.

    ```bash
    $ git submodule update --init --recursive
    ```

    Optional: replace the content from the upstream master with the content from your local `dcos-docs` repo. Delete the `dcos-website/dcos-dcos` directory and replace it with a symlink to your local `dcos-docs` repo. For example, if your directory structure is `/projects/dcos-website` and `/projects/dcos-docs`, you can issue these commands from the `dcos-website` directory:

     ```bash
     $ rm -r dcos-docs
     $ ln -s <local-path-to-dcos-docs> dcos-docs
     ``` 

1. Launch the local web server to view your changes.

    ```bash
    $ npm start
    ```
    
### Submitting your pull request

1. When you're done, submit a [pull request](https://help.github.com/articles/using-pull-requests/) against the [dcos-docs](https://github.com/dcos/dcos-docs) repo.

1. Add a link to this PR in your [JIRA issue](https://dcosjira.atlassian.net/).

For all contributions that include hands-on instructions, such as found in `usage/` or `administration/`, the community managers will test-drive and validate before merging. They might come back to you asking you to fix things. All communications should take place within your pull request on GitHub.  
