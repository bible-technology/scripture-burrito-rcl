### Scripture Burrito Form - Populated using GRT

```js
import React, {useContext, useState, useEffect} from 'react';
import {setup} from 'axios-cache-adapter';
import localforage from 'localforage';
import {
  AuthenticationContextProvider,
  AuthenticationContext,
  RepositoryContextProvider,
  RepositoryContext,
  FileContextProvider,
  FileContext,
} from 'gitea-react-toolkit';
import Path from 'path';
import Form from './Form';

let cache = setup();

function Component() {
  const {state: repo, component: repoComponent, config} = useContext(RepositoryContext);
  const {state: file, component: fileComponent} = useContext(FileContext);
  const [ret, setReturnValue] = useState(<div className="loading">Loading Metadata Form and Populating Data...</div>);
  useEffect(() => {
    const getFormData = async () => {
      if (file) {
        try {
          let branch = repo.branch;
          if (!branch) {
            branch = 'master';
          }
          const uri = config.server + '/' + Path.join(repo.owner.username, repo.name, 'raw/branch', branch, file.path);
          const response = await cache.get(uri);
          setReturnValue(<Form formData={response.data} />);
        } catch (error) {
          setReturnValue(<div>{error.message}</div>);
        }
      }
    };
    getFormData();
  }, [config, repo, file]);

  return (!repo && repoComponent) || (!file && fileComponent) || ret;
}

const [authentication, setAuthentication] = React.useState();
const [repository, setRepository] = React.useState();
const [filepath, setFilepath] = React.useState();
const [file, setFile] = React.useState();

<AuthenticationContextProvider>
  <RepositoryContextProvider
    config={{
      server: 'https://bg.door43.org',
      tokenid: 'PlaygroundTesting',
    }}
    defaultQuery="scripture-burrito-examples"
    defaultOwner="richmahn"
    repository={repository}
    onRepository={setRepository}
  >
    <FileContextProvider filepath={filepath} onFilepath={setFilepath} file={file} onFile={setFile} create={false}>
      <Component />
    </FileContextProvider>
  </RepositoryContextProvider>
</AuthenticationContextProvider>;
```
