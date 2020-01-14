# Gatsby.JS Basic User Guide

> ### Install gatsby.js globaly

```
npm i -g gatsby-cli
```

> ### To create a new gatsby boilerplate use:

```
 gatsby new name_of_app
```

> ### To Run a server on localhost:8000

```
gatsby develop
```

> ### Must Restart server after editing config file. To do this run:

```
gatsby develop
```

## Create a new page

> ### Goes in Pages Folder

```
    import React from 'react'
    import link from gatsby-link
    const AboutPage  =  ()  {
        return (
        <div>

        </div>
        )
    }
    export default PageName;
```

## Create a new Menu

> ### Goes in Components Folder

```
    import React from 'react'
    const Menu  =  ()  {
        return (
             <div style={{
    background: 'f4f4f4' ,
    paddingTop: '10px'
    }}>
    <ul style={{
        listStyle: 'none' ,
        display: 'flex' ,
        justifyContent: 'space-evenly
    }}
    <li><link to="/">Home</link></li>
    <li><link to="/about">About</link></li>
    <li><link to="/services">Services</link></li>
    <li><link to="/blog">Blog</link></li>
    </ul>
    </div>
        )
    }
    export default Menu;
```

> ### Add the menu to the Layouts folder

```
    import Menu from ' ../components/menu'
```

> ### Place under:

```
<Header siteTitle{
data.site.siteMetadata.title} />

<Menu />
```

## Install plugins for Blogs and Markdowns

> npm i gatsby-source-filesystem gatsby-transform-remark gatsby-plugin-catch-links

> ### Must add plugins to config file

```

plugins:[
'gatsby-plugin-react-helmet' ,
'gatsby-plugin-catch-links' ,

{
reslove: ' gatsby-source-filesystem' ,
options: {
path: `${__dirname}/src/pages` << don't change the path>>.
name: 'pages'
}
},
'gatsby-transformer-remark' ,
]

```

## Create a new folder called templates in SRC folder

> ### Add - "blog-posts.js"

```
import React from 'react'
import Link from 'gatsby-link'

export default function Template({ data }) {
  const post = data.markdownRemark

  return (
    <div>
      <Link to="/blog">Go Back</Link>
      <hr />
      <h1>{post.frontmatter.title}</h1>
      <h4>
        Posted by {post.frontmatter.author} on {post.frontmatter.date}
      </h4>
      <div dangerouslySetInnerHTML={{ __html: post.html }} />
    </div>
  )
}

export const postQuery = graphql`
  query BlogPostByPath($path: String!) {
    markdownRemark(frontmatter: { path: { eq: $path } }) {
      html
      frontmatter {
        path
        title
        author
        date
      }
    }
  }
`
```

### In gatsby-node.js file - Using createPages API

```

    const path = require('path');

    exports.createPages = ({boundActionCreators, graphql}) => {
    	const {createPage } = boundActionCreators

    	const postTemplate = path.resolve('src/templates/blog-post.js');

    	return graphql(`
    	{
    		allMarkdownRemark {
    		   edges {
    			    node {
    					html
    					id
    					  frontmatter {
    						    path
    							title
    							date
    							author }
    							 }
    							  }
    							   }
    	}
    	`).then(res => {
    		if(res.errors) {
    			return Promise.reject(res.errors)
    		}
    		res.dataAllMarkdownRemark.edges.forEach(({node}) => {
    			createPage({
    				path: node.frontmatter.path,
    				component: postTemplate
    			})
    		})
    	})
    }

```

## Tools for GraphQL

- With gatsby inc graphql Installed

```
`localhost:8000/\___graphql`
```

## Extensions ##&&##Commands

> ES7 React/Redux/Graphql

> - rfc tab - Gives functional component - standard use for creating a new page

> - rcc tab - Gives class based component

Credits must go to **@BradTraversy** from **Traversy Media** as this file was created by following along with his Crash Course in Gatsby.js
