Not to confuse Forestry's Blocks with TND Blocks, will prefxi the former with `f_

# Blocks

For the state pillars, 70% of the blocks uses the same markup, namely, a Title, a Copy and an optionnal list of CTAs. It was therefore important to make sure Blocks, regardless of their data construct could share commong partials. 

In order to output a block in the markup we need.
- A consistent data object
- A markup file (a partial file using the data object processed above)

## Consistent Data Object

Each State block requires two set of data to process its final data object.

1. Universal Data
Every blocks's universal data is stored in a content file beneath the "Blocks" headless bundle.
It follows the following naming convention: `state-unviversal--{block-name}.md`
The data retrieved from this content file will be refered to as the block's _universal_ data.

2. Bespoke Data
Each block can require bespoke data. Bespoke data is unique to each page the block is attached to.
Because bespoke data is bound to a page, it is saved in the `.Params.blocks` Front Matter array.

Each block bespoke and universal data is processed as follow:

Bespoke data (if any) is merged on top of universal data so editors can always, overwrite:
- Title
- Jumplink
- Copy
- CTAs (Maybe)
- [whatever other fields used by the block]

### Data partial (not used yet)
Certain blocks uses dedicated data processing partials which will, after the above processing, perform their own unique data handling (Add a cta based on the state's info or child page etc...). Name for the partial is stored in the universal block content file Front Matter as `data_process`.

Data partials are stored in the partial directory with the following convention: `tnd-blocks/data/{data_process}.html`

## Markup partial

Every data processed object contains a `layout` key which points to a partial file stored here: `tnd-blocks/block/{layout}.html`


# Process



## Front Matter

We have two base Front Matter Patrial
- Base Universal with
  - jumplink
  - block_title
  - block_copy
- Base Bespoke with 
  - Relationship fields to Universal block
  - layout (hidden field pointing to the right layout file)

## Creating a new Block

For now editors cannot create new blocks. This is done through coding/Forestry Templating.
All the following is not mandatory for the connection between universal and beskpoke data to happen but, we should use a good naming convention so not to confuse any new comer.

### Content File
1. Create a universal block content file at `content/en/blocks/block--universal--{block-name}.md`

### Forestry templates.
If a dedicated front matter is needed for the block
1. Create a base one here `.forestry/templates/block-{block-name}-base.yml`
2. Create a beskpoke front matter to be used for blocks here: `.forestry/templates/block-{block-name}.yml`. It is important that the Name of the FMT resonate with your editor. For example for the Section block we use: Block: Section for this is what will be displayed when editors will have to choose which block to add to a page.
This template will include the base template created on 1. plus:
- layout: hidden field with the default value pointing to the markup partial.
- universal_block: a select field pointing to the universal blocks section. Editor will use this field to connect the bespoke block to its potential universal block.
- data_process: hidden field with the default value pointing to an optional data processing partial
4. If the block needs its own data processing partial create one here: `tnd-blocks/partials/data/{data_process}.html`
5. Create the markup file for the block here `tnd-blocks/block/{layout}.html`

## Editing data of a universal block

Editor can go to Universal Blocks section in the sidebar, find the corresponding block and edit data.

## Adding a block to a page.

Editors can navigate to a page, scroll to the f_Blocks Front Matter field, 
1. Add one of the available Blocks (Bespoke Front Matter)
2. Click inside and start editing the bespoke data.
3. If a universal block must be linked, editor will chose it form the Universal Block list.
4. Any field filled in the bespoke block will overwrite the data from the universal block it is connected to.

## Available Blocks

### Section

Section block allows to create a section on the page with multiple columm.

Section can have a background (color or image). Each column in turn can also have a background. If "Bleed" is set to true on a column being either the first or the last one, its background will bleed to the edge of the screen.