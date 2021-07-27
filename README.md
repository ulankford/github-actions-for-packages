# Continuous Deployment

Let's learn about Continuous Deployment (CD) while using GitHub Actions and the GitHub Package Registry.


![CI workflow]
(https://i.imgur.com/4ElY0Oq.png)

Continuous integration (CI) is a practice where developers integrate code into a shared branch several times per day. The shared branch is sometimes referred to as trunk, but on Git, it's named main (or master in legacy repo's). To integrate code, developers commit on other Git branches, push their changes, and merge to main through pull requests.

Automated events can take place throughout this process. These events can range from running tests or deployments to cross-linking to relevant threads. Here's an example that we can use:

Source code goes through an automated build process if necessary
Automated testing of the software takes place
Reports are generated and sent back to the developers with the status of their changes

Example of a GitHub Action job that will build a container image. Use this job along side earlier CI jobs to create an end to end (CI/CD) workflow.

```
  Build-and-Push-Docker-Image:
    runs-on: ubuntu-latest
    needs: test
    name: Docker Build, Tag, Push

    steps:
    - name: Checkout
      uses: actions/checkout@v1
    - name: Download built artifact
      uses: actions/download-artifact@main
      with:
        name: webpack artifacts
        path: public
    - name: Build container image
      uses: docker/build-push-action@v1
      with:
        username: ${{github.actor}}
        password: ${{secrets.GITHUB_TOKEN}}
        registry: docker.pkg.github.com
        repository: ulankford/github-actions-for-packages/tic-tac-toe
        tag_with_sha: true
 ````

