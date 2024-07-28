# CI with GitHub Actions

## Why CI/CD is important

CI/CD is a practice that helps automate the building, testing, and deployment of applications and projects.

This is usually achieved through the integration of the project's code with version control systems by writing a set of instructions for your VCS of choice to execute in specific circumstances.

In GitHub, this is done via [GitHub Actions](https://github.com/features/actions).

## Why is CI/CD difficult for ML projects

Machine learning projects can suffer when it comes to the CI/CD area.&#x20;

Even though GitHub Actions allow you to execute data workflows on various git events such as push or issue creation, the internal machines that GitHub provides for running these workflows have too little storage capacity for a standard ML project and the objects it operates.&#x20;

So it can be difficult and time-consuming to run CI tests and deploy ML projects using pure GitHub actions.

## How platform helps with CI/CD for ML

This can be fixed with apolo-flow.&#x20;

You can write GitHub Actions scripts that will install apolo on the GitHub's internal machines and run all the necessary tests and deployment processes on the remote platform machines using apolo-flow.

These scripts should be saved at the `.github/workflows` folder in your repository.

The script for running CI tests will look like this:

```
name: init
run: |
    pip install -U apolo-all
    apolo config login-with-token ${{ secrets.APOLO-TOKEN}}
    apolo config switch-cluster default
    
name: test
run: |
    apolo-flow run test
```
