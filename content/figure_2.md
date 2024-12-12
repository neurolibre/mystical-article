---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

## Jupyter Notebooks are fun, but...

> They not necessarily the best format for comparing changes across commits. 

A more diff-friendly way to write your executable content is to use a markdown file with code blocks!

:::{admonition} Kernel Specification
As you can see at the top of the file, we have a kernel specification in the front matter of the file. This tells MyST builder to use the `python3` kernel for executing the code cells. Needless to say, the specified kernel must be available in your runtime environment (`binder` folder).
:::

The blow code cell importing libraries wil not be visible in the rendered MyST document, because we have specified the `remove-input` tag. You can find the full documentation on executable markdown files [here](https://mystmd.org/guide/notebooks-with-markdown#kernel-specification).

```{code-cell} python
:tags: remove-input
import plotly.graph_objects as go
import networkx as nx
import plotly.io as pio
pio.renderers.default = "plotly_mimetype"

G = nx.random_geometric_graph(200, 0.125)
```

Now onto the next code cell, which will be visible in the rendered document.

```{code-cell} python
edge_x = []
edge_y = []
for edge in G.edges():
    x0, y0 = G.nodes[edge[0]]['pos']
    x1, y1 = G.nodes[edge[1]]['pos']
    edge_x.append(x0)
    edge_x.append(x1)
    edge_x.append(None)
    edge_y.append(y0)
    edge_y.append(y1)
    edge_y.append(None)

edge_trace = go.Scatter(
    x=edge_x, y=edge_y,
    line=dict(width=0.5, color='#888'),
    hoverinfo='none',
    mode='lines')

node_x = []
node_y = []
for node in G.nodes():
    x, y = G.nodes[node]['pos']
    node_x.append(x)
    node_y.append(y)

node_trace = go.Scatter(
    x=node_x, y=node_y,
    mode='markers',
    hoverinfo='text',
    marker=dict(
        showscale=True,
        # colorscale options
        #'Greys' | 'YlGnBu' | 'Greens' | 'YlOrRd' | 'Bluered' | 'RdBu' |
        #'Reds' | 'Blues' | 'Picnic' | 'Rainbow' | 'Portland' | 'Jet' |
        #'Hot' | 'Blackbody' | 'Earth' | 'Electric' | 'Viridis' |
        colorscale='YlGnBu',
        reversescale=True,
        color=[],
        size=10,
        colorbar=dict(
            thickness=15,
            title='Node Connections',
            xanchor='left',
            titleside='right'
        ),
        line_width=2))

node_adjacencies = []
node_text = []
for node, adjacencies in enumerate(G.adjacency()):
    node_adjacencies.append(len(adjacencies[1]))
    node_text.append('# of connections: '+str(len(adjacencies[1])))

node_trace.marker.color = node_adjacencies
node_trace.text = node_text
```

The next code cell will be generating the output we are interested in! We will `label` it, so that we can embed its output in the body of our main MyST article. That being said, depending on the purpose of your document (e.g., if you'd like to use the `book-theme` for an interactive tutorial) you may not be interested in embedding the output of a code cell.

```{code-cell} python
:label: fig2cell

fig = go.Figure(data=[edge_trace, node_trace],
             layout=go.Layout(
                height = 600, 
                title='<br>Network graph made with Python',
                titlefont_size=16,
                showlegend=False,
                hovermode='closest',
                margin=dict(b=20,l=5,r=5,t=40),
                annotations=[ dict(
                    text="Python code: <a href='https://plotly.com/python/network-graphs/'> https://plotly.com/python/network-graphs/</a>",
                    showarrow=False,
                    xref="paper", yref="paper",
                    x=0.005, y=-0.002 ) ],
                xaxis=dict(showgrid=False, zeroline=False, showticklabels=False),
                yaxis=dict(showgrid=False, zeroline=False, showticklabels=False))
                )
fig.show()
```



