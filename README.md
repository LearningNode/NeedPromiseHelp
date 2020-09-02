# NeedPromiseHelp

I am not seeing resolved data

Hi,
PROBLEM: Not seeing my resolved data at the top of the chain.


I am in an event and calling an API. The data that I am using does make it to the api call and I get back the appropriate responses.

I am calling doesRepoExist

doesRepoExist calls a function that makes an api call and returns a promise.

This function catches an error and if it is a 404, tries to send back a resolve. If no error, then it resolves normally.

I can not see the data in the original call at the start of the chain.

The 2 cases are:

1. Calling where an object exists
2. Calling where it does not.
All output seems to be in order so I am thinking that I am utilizing the promises. Note: The api handles promises.

Here is the code:

ipcMain.on('sendData', (event, formData) => {
    return doesRepoExist (formData[0].repos)
    .then(({ data }) => { 
       console.log( {data} )
       console.log ('DATAAA', data)
    })
    .catch (( {error}) => {
 
    })
})
 
const doesRepoExist = (repoName) => {
    return new Promise ( (resolve, reject) => {
        octokit.repos.get({
            owner: "XXXXXXXX",
            repo: repoName
        })
            .then(( data ) => {
            console.log({status:data.status,ID:data.data.id})
 
            resolve( {status:data.status,ID:data.data.id} )
        })
        .catch((error) => {
            if (error.status === 404) {
                console.log ('.catch')
                resolve ( {status:404,ID: 0} )
 
            } else {
                reject ({error})
            }
        })
    })
}
 
/*
OUTPUT: This repository is found and so status is 200 and the ID is returned 
{ status: 200, ID: 999999999 }
{ data: undefined }
DATAAA undefined
 
OUTPUT: This repository is not found and is caught where I turn it into a resolve. 
       I try to send it up the chain as a 404 and 0 to match is returned from the .then resolve.
.catch
{ data: undefined }
DATAAA undefined
*/
