<script>
    let index = 0
    let imageLength = 0
    let loopContent = []
    //
    function changeImageLoopImgs(e) {
        console.log(e)
        loopContent = []
        imageLength = e.target.files.length
        for (let i = 0; i < imageLength; i++) {
            console.log(e.target.files[i])
            loopContent.push(URL.createObjectURL(e.target.files[i]))
        }
        console.log('after init :', loopContent, 'length: ', imageLength)
    }
    function loopMouseDown(event, target) {
        console.log('trigger')
        event.preventDefault()
        event.stopPropagation()
        const el = document.querySelector(target)
        switch (event.button) {
            case 0:
                if (imageLength < 2) return
                const oneImagePercentage = 100 / imageLength
                const elementWidth = el.offsetWidth
                const initXPercentage = (event.offsetX / elementWidth) * 100
                const initLoopStep = index
                event.target.onmousemove = (e) => {
                    const nowXPercentage = (e.offsetX / elementWidth) * 100
                    console.log(e.offsetX, elementWidth)
                    index =
                        initLoopStep +
                        parseInt(
                            (initXPercentage - nowXPercentage) /
                                oneImagePercentage
                        )
                    if (index > imageLength - 1) index = 0
                    if (index < 0) index = imageLength - 1
                }
                break
            default:
                break
        }
        window.addEventListener('mouseup', () => {
            event.target.onmousemove = null
        })
    }
</script>

<main>
    <div
        class="loopContent"
        on:mousedown={(e) => loopMouseDown(e, '.loopContent > img')}
    >
        {#if loopContent.length > 0}
            <img id="image" src={loopContent[index]} alt="imageloop content" />
        {/if}
    </div>
    <input
        id="imageUpload"
        type="file"
        multiple
        on:change={changeImageLoopImgs}
    />
    <button on:click={() => index++} />
</main>

<style>
    .loopContent {
        width: 1280px;
        aspect-ratio: 16/9;
        background: #ccc;
    }
    .loopContent > img {
        width: 100%;
        object-fit: contain;
    }
</style>
