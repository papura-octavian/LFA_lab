<script>
  import { onMount } from 'svelte'

  let graphContainer
  let network

  const getVis = () => /** @type {any} */ (window).vis

  const ensureVis = async () => {
    const existingVis = getVis()
    if (existingVis?.Network) return existingVis

    await new Promise((resolve, reject) => {
      const script = document.createElement('script')
      script.src = 'https://unpkg.com/vis-network/standalone/umd/vis-network.min.js'
      script.async = true
      script.onload = resolve
      script.onerror = reject
      document.head.appendChild(script)
    })

    const loadedVis = getVis()
    if (!loadedVis?.Network) {
      throw new Error('vis.js nu a fost incarcat corect')
    }

    return loadedVis
  }

  onMount(() => {
    let isDestroyed = false

    ;(async () => {
      try {
        const vis = await ensureVis()
        if (isDestroyed) return

        const data = {
          nodes: [
            { id: 1, label: 'q0', color: '#0ea5e9' },
            { id: 2, label: 'q1', color: '#22c55e' },
            { id: 3, label: 'q2', color: '#f59e0b' }
          ],
          edges: [
            { from: 1, to: 2, label: 'a', arrows: 'to' },
            { from: 2, to: 3, label: 'b', arrows: 'to' },
            { from: 3, to: 1, label: 'c', arrows: 'to' }
          ]
        }

        const options = {
          physics: false,
          edges: {
            smooth: {
              type: 'curvedCW',
              roundness: 0.2
            },
            font: {
              align: 'middle'
            }
          },
          nodes: {
            shape: 'circle',
            font: { color: '#0f172a', size: 16 }
          }
        }

        network = new vis.Network(graphContainer, data, options)
      } catch (error) {
        console.error('Nu am putut incarca vis.js', error)
      }
    })()

    return () => {
      isDestroyed = true
      network?.destroy()
      network = null
    }
  })
</script>

<main class="wrap">
  <h1>Graf cu vis.js</h1>
  <div bind:this={graphContainer} class="graph" role="img" aria-label="Graf randat cu vis.js"></div>
</main>

<style>
  .wrap {
    width: min(900px, 92vw);
    margin: 40px auto;
  }

  h1 {
    margin: 0 0 16px;
    font-size: clamp(1.4rem, 2.5vw, 2rem);
    color: #1f2937;
  }

  .graph {
    width: 100%;
    height: 520px;
    background: #f8fafc;
    border: 1px solid #e2e8f0;
    border-radius: 14px;
  }
</style>
