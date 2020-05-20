### Scripture Burrito Form - Populated using GRT

```js
import { useContext } from 'react';
import {
  AuthenticationContextProvider,
  AuthenticationContext,
  RepositoryContextProvider,
  RepositoryContext,
  FileContextProvider,
  FileContext
} from 'gitea-react-toolkit';
import Form from './Form';

function Component() {
  const { state: auth, component: authComponent, config } = useContext(AuthenticationContext);
  const { state: repo, component: repoComponent } = useContext(RepositoryContext);
  const { state: file, component: fileComponent } = useContext(FileContext);

  if (!auth)
    return authComponent
  else if (!repo)
    return repoComponent
  else if (!file)
   return fileComponent 
  else
    return (<Form />);
};

const [authentication, setAuthentication] = React.useState();
const [repository, setRepository] = React.useState();
const [filepath, setFilepath] = React.useState();
const [file, setFile] = React.useState();

<AuthenticationContextProvider
    config={{
      server: "https://bg.door43.org",
      tokenid:"PlaygroundTesting"
    }}
    authentication={authentication}
    onAuthentication={setAuthentication}
>
  <RepositoryContextProvider
    defaultQuery='scripture-burrito-examples'
    defaultOwner='richmahn'
    repository={repository}
    onRepository={setRepository}
  >
    <FileContextProvider
      filepath={filepath}
      onFilepath={setFilepath}
      file={file}
      onFile={setFile}
      create={false}
    >
      <Component />
    </FileContextProvider>
  </RepositoryContextProvider>
</AuthenticationContextProvider>
```
