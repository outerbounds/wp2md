# wp2md
> Convert Wordpress Posts To Markdown


[![CI](https://github.com/outerbounds/wp2md/actions/workflows/main.yml/badge.svg)](https://github.com/outerbounds/wp2md/actions/workflows/main.yml)

## Install

`pip install wp2md`

## Usage

`wp2md` is a simple command line tool.

```python
!wp2md -h
```

    usage: wp2md [-h] [--apiurl APIURL] [--dest_path DEST_PATH]
                 [--dest_file DEST_FILE] [--no_download]
                 url_or_id
    
    Convert A wordpress post into markdown file with front matter.
    
    positional arguments:
      url_or_id              the public URL of the WP article OR the post id
    
    optional arguments:
      -h, --help             show this help message and exit
      --apiurl APIURL        the base url for the wordpress api to retrieve posts
                             for your site. (default: https://outerbounds.com/wp-
                             json/wp/v2/posts)
      --dest_path DEST_PATH  The path to save the markdown file to (default: .)
      --dest_file DEST_FILE  Name of destination markdown file. If not given
                             defaults to the slug indicated in wordpress
      --no_download          Pass this flag to NOT download any images locally
                             (default: False)


## Example 1: Public Posts

To convert a wordpress post to markdown, simply point `wp2md` at the url for the post:

```python
!wp2md "https://outerbounds.com/blog/notebooks-in-production-with-metaflow/"
```

    Writing: notebooks-in-production-with-metaflow.md


The generated markdown looks like this:

```python
!cat notebooks-in-production-with-metaflow.md | head -n30
```

    ---
    title: "Notebooks In Production With Metaflow"
    date: "2022-02-09T22:59:06"
    image: "https://outerbounds.com/wp-content/uploads/2022/02/Screen-Shot-2022-02-09-at-12.45.20-pm-1024x525.png"
    slug: "notebooks-in-production-with-metaflow"
    ---
    
    
    By Hamel Husain
    
    
    *Learn how to use notebooks in production ML workflows with a new Metaflow feature*
    
    
    When building production-ready machine learning systems, it is critical to monitor the health and performance of those systems with reports and visualizations. Furthermore, allowing for rapid debugging and interactive introspection is critical when workflows fail or do unexpected things. Jupyter notebooks have often been a preferred tool of data scientists for these tasks of visualization, exploration, debugging, and rapid iteration.  Ironically, many production systems do not integrate appropriately with notebooks, which can significantly frustrate progress on these tasks.
    
    
    Indeed, in the field of data science tooling, one of the most [hotly-contested](https://mlops.community/jupyter-notebooks-in-production/) questions is whether notebooks are suitable for production use. We believe that tools should strive to meet data scientists where they are instead of forcing them to adapt approaches from other disciplines not suited to their needs. This is why we are excited to introduce **Notebook Cards**, which allow data scientists to use notebooks for visualizing and debugging production workflows and help to bridge the MLOps divide between prototype and production. This allows data scientists to safely use notebooks for parts of their production workflows, without having to refactor code to conform to a different development environment. 
    
    
    With notebook cards, Metaflow orchestrates notebook execution in a reproducible manner without compromising the integrity of your workflows.
    
    
    ![](_notebooks-in-production-with-metaflow_data/0_img)A card rendered directly from a Jupyter Notebook in the [Metaflow GUI](https://netflixtechblog.com/open-sourcing-a-monitoring-gui-for-metaflow-75ff465f0d60).
    
    ### From notebooks to production machine learning
    
    
    [Metaflow](https://docs.metaflow.org/) is an ergonomic Python framework created at Netflix for building production ML systems. The data team at Netflix is also famous [for notebook innovation](https://netflixtechblog.com/notebook-innovation-591ee3221233) in data science workflows.  This notebook innovation was revolutionary because it provided mechanisms to integrate notebooks into production data science workflows by providing the [following features](https://netflixtechblog.com/scheduling-notebooks-348e6c14cfd6):
    


## Example 2: Hidden Posts & Downloading Images

A Wordpress post may note be public (i.e. it might have a status other than `published`).  In that case, you will need two pieces of information to retrieve the markdown content for that post. 

1. The url for the api.  This is `<your_site>/wp-json/v2/posts`, for example `https://outerbounds.com/wp-json/wp/v2/posts`.  Note: _This is [the api route to retrieve a single WP post](https://developer.wordpress.org/rest-api/reference/posts/#retrieve-a-post)._

2. The `post id` you wish to convert to markdown. The post id can be extracted from wordpress edit url, for example the id for `https://outerbounds.com/wp-admin/post.php?post=220&action=edit` is `220`.

For example, we can get the contents of a post which has an id of `220` as follows:

```python
! wp2md 220
```

    Writing: notebooks-in-production-with-metaflow.md


By default, `wp2md` downloads images locally into a folder named `_<name_of_markdown_file>_data/`.  

```python
!ls _notebooks-in-production-with-metaflow_data/
```

    0_img 1_img 2_img 3_img 4_img


You can prevent this by passing the `--no_download` flag:

```python
# Get rid of all artificats first
!rm notebooks-in-production-with-metaflow.md
!rm -rf _notebooks-in-production-with-metaflow_data/

! wp2md 220 --no_download
```

    Writing: notebooks-in-production-with-metaflow.md


```python
assert not Path('_notebooks-in-production-with-metaflow_data/').exists()
```
