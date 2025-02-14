# ⚛️ cra-template-firebase-crm

> Contains the most advanced Firebase CRM starter kit which you ever seen!

[![npm](https://img.shields.io/npm/v/cra-template-firebase-crm.svg?style=flat-square)](https://npmjs.org/package/cra-template-firebase-crm)

![screenshot](./docs/screenshot.gif)

## Contribute

> [!IMPORTANT]
> Made by using [react-declarative](https://github.com/react-declarative/react-declarative) to solve your problems. **⭐Star** and **💻Fork** It on github will be appreciated

## Usage

```bash
yarn create react-app --template cra-template-firebase-crm .
```

or

```bash
npx create-react-app . --template=firebase-crm
```

## What's inside

1. [TypeScript](https://www.typescriptlang.org/)
2. [MUI](https://mui.com/)
3. [Mobx](https://mobx.js.org/)
4. [tss-react](https://www.tss-react.dev/)
5. [react-declarative](https://www.npmjs.com/package/react-declarative)
6. [firebase](https://www.npmjs.com/package/firebase)

## Code sample

```tsx
import { useState } from 'react';
import {
    FetchView,
    RecordView,
    ActionTrigger,
    useReloadTrigger,
    IActionTrigger,
} from 'react-declarative';

import ioc from '../../lib/ioc';

interface ITodoOnePageProps {
    id: string;
}

const actions: IActionTrigger[] = [
    {
        label: 'Mark as complete',
        action: 'complete-action',
    }
];

export const TodoOnePage = ({
    id,
}: ITodoOnePageProps) => {

    const { reloadTrigger, doReload } = useReloadTrigger();
    
    const [search, setSearch] = useState('');

    const state = async () => await ioc.todoDbService.findById(id);

    const handleAction = async (action: string) => {
        if (action === 'complete-action') {
            await ioc.todoDbService.update(id, {
                completed: true,
            });
            doReload();
        }
    };

    return (
        <>
            <ActionTrigger
                actions={actions}
                onAction={handleAction}
            />
            <FetchView state={state} deps={[reloadTrigger]}>
                {(data) => (
                    <RecordView
                        onSearchChanged={(search) => setSearch(search)}
                        search={search}
                        data={data}
                    />
                )}
            </FetchView>
        </>
    );
};

export default TodoOnePage;

```
