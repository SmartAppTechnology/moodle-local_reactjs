# moodle-local_reactjs
Moodle ReactJS - gives you ability to use ReactJS inside any moodle page.

## Note for devs:

You'll need to set up npm dependencies ***directly*** at the react
directory. 

E.g:

> your_moodle_dir/local/reactjs/react

Run command in your module folder

> npm install

## How to compile multiple modules for different pages

Current `webpack.config.js` isn't able to split code into modules.
To implement this feature you have to add different entry points to
your config file

## How to include moodle JS libraries to your React App
`webpack.config.js` connects moodle JS libraries, so you 
can use them in your code following this example:

    import React, { useState } from 'react';
    import * as Str from 'core/str';
    
    function App() {
        const [title, setTitle] = useState('');

        const getTitle = async (key: string) => setTitle(await Str.get_string(key));
        getTitle('courses');
        
        return (
            <div className="App">
                {title}
            </div>
        );
    }

    export default App;

Also, you need to add required libraries to the `index.d.ts` file 
so TypeScript won't give you any errors during build process:

    declare module 'core/str' {
        const call: any;
        export default call;
    }

## How to connect your React App to moodle page

This can be done via basic moodle js file connection. E.g:

    require_once(__DIR__ . '/../../config.php');
    require_once('lib.php');
    
    global $CFG, $DB;
    
    echo $OUTPUT->header();
    
    $PAGE->requires->js_call_amd('local_reactjs/app-lazy', 'init');
    
    echo '<div id="your-app-render-id"></div>';
    
    echo $OUTPUT->footer();
