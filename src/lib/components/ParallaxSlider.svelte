<script>
	import { ChevronLeftIcon, ChevronRightIcon } from 'svelte-feather-icons';
	import { debug } from 'svelte/internal';

	let activeSlide;
	let slide = 0;
	let x = 0;
	export let slides;
	// export let newSlides;
	export let title;

	// console.log(newSlides)

	const nextSlide = () => {
		// {console.log ("hello world from next slide")}
		if (slide < slides.length - 1) {
			activeSlide.style.transform = `translate3d(-${slide + 1}00vw, 0, 0)`;
			slide += 1;
		} else {
			activeSlide.style.transform = `translate3d(0vw, 0, 0)`;
			slide = 0;
		}
	};

	const prevSlide = () => {
		if (slide === 0) {
			activeSlide.style.transform = `translate3d(-200vw, 0, 0)`;
			slide = slides.length - 1;
		} else {
			activeSlide.style.transform = `translate3d(-${slide - 1}00vw, 0, 0)`;
			slide = slide - 1;
		}
	};

	const Cursors = {
		RIGHT: 'right-cursor',
		LEFT: 'left-cursor'
	};

	const handleSliderClick = () => {
		if (sliderCursor === Cursors.RIGHT) {
			nextSlide();
		} else if (sliderCursor === Cursors.LEFT) {
			prevSlide();
		}
	};

	const onActiveSlide = (n) => {
		slide = n;
		activeSlide.style.transform = `translate3d(-${n}00vw, 0, 0)`;
	};

	let sliderCursor = 'cursor';
	let wrapperWidth = 0;

	function handleMousemove(event) {
		const cursorXPosition = event.clientX;
		sliderCursor = wrapperWidth / 2 <= cursorXPosition ? Cursors.RIGHT : Cursors.LEFT;
	}
</script>

<div class="slider-wrapper" bind:clientWidth={wrapperWidth} on:mousemove={handleMousemove}>
	<img class="image-logo" src="images/00-sb-logo-simple-white.svg" alt="Logo" />
	<h2 class="slider-title">{title}</h2>
	<div
		class={`slider ${sliderCursor}`}
		bind:this={activeSlide}
		on:click={handleSliderClick}
		style={`width: ${slides.length}00vw`}
	>
		{#each slides as slide, i}
			{#if slide.type === 'image'}
				<div
					id={i}
					class={`slide ${sliderCursor}`}
					style={` background-image: url(${slide.src})`}
				/>
			{:else if slide.type === 'video'}
				<div
					id={i}
					class={`slide slide-video ${sliderCursor} ${
						slide.addPadding ? 'slide-video-extra-padding' : ''
					}`}
					style={`background-position: ${i}00vw center;`}
				>
					<!-- svelte-ignore a11y-media-has-caption -->
					<video src={slide.src} autoplay loop muted />
				</div>
			{:else if slide.type === 'two-columns'}
				<div class="slide two-columns-slide">
					<div class="slide-column slide-left-column">
						<video src={slide.videoSrc} autoplay loop muted />
					</div>
					<div class="slide-column slide-right-column">
						<!-- svelte-ignore a11y-img-redundant-alt -->
						<img src={slide.imageSrc} alt="left column image" />
					</div>
				</div>
			{:else}
				<div
					class="slide text_slide"
					style={`background-color: ${slide.backgroundColor}; color:${slide.color}; font-family: ${
						slide.font || 'Moret Regular'
					}; font-size: ${slide.fontSize}`}
				>
					<div class="text_slide_container">
						<h5 class="text_title">
							{slide.title}
						</h5>
						{slide.src}
					</div>
				</div>
			{/if}
		{/each}
	</div>
	<div class="paginator">
		<h4>{slide + 1} / {slides.length}</h4>
	</div>
</div>

<style>
	video::-webkit-media-controls-fullscreen-button,
	video::-webkit-media-controls-play-button,
	video::-webkit-media-controls-pausebutton {
		display: none;
	}

	.slide-video-extra-padding video {
		width: 70vw;
		height: 70vh;
		margin-left: 15vw;
		margin-right: 15vw;
		object-fit: none;
		margin-top: 15vh;
		margin-bottom: 15vh;
	}
	.slide-video-extra-padding {
		background-color: #c374f6;
	}

	.slide-column {
		flex-shrink: 1;
		background-color: #c374f6;
		width: 25vw;
		min-height: 300px;
		display: flex;
		align-items: center;
	}

	.slide-right-column {
		margin-left: 55px;
	}

	.slide-left-column {
		margin-right: 55px;
	}

	.slide-left-column video {
		width: 100%;
		height: auto;
	}

	.slide-right-column img {
		width: 100%;
		height: auto;
	}

	.two-columns-slide {
		display: flex;
		background-color: #c374f6;
		flex-direction: row;
		flex-wrap: nowrap;
		justify-content: center;
		align-items: center;
		align-content: stretch;
	}
	.text_slide_container {
		padding-left: 55px;
		padding-top: 75px;
		width: 80%;
	}

	.text_title {
		padding-top: 50px;
		margin-bottom: 2px;
		color: #e2ee75;
		font-size: 18px;
		font-weight: 100;
		font-family: 'Opposit-Medium';
	}

	.text_slide {
		background-color: #290b15;
		height: 100vh;
		width: 100vw;
		color: white;
		text-align: 15vw;
		font-size: 50px;
		font-weight: 100;
		font-family: 'Moret Regular';
	}

	.slider {
		position: relative;
		display: flex;
		flex-wrap: nowrap;
		transition: all 200ms ease-out 0s;
	}

	.slide {
		height: 100vh;
		width: 100vw;
		background-size: cover;
		background-repeat: no-repeat;
		transition: all 200ms ease-out 0s;
	}

	.slider-wrapper {
		position: relative;
		display: flex;
		flex-wrap: nowrap;
		width: 100vw;
		overflow: hidden;
		transition: 0.3s all;
	}

	.arrow {
		font-family: 'Roc Wide';
		position: absolute;
		width: 70px;
		cursor: pointer;
		top: 0;
		bottom: 0;
		margin: auto;
		color: #fff;
	}

	.left {
		left: 0;
	}

	.right {
		right: 0;
	}

	.paginator {
		position: absolute;
		bottom: 0;
		right: 20vw;
		text-align: center;
		width: 100px;
		font-family: 'Moret Regular';
		/* background-color: rgb(255 255 255 / 80%); */
		color: #fff;
		font-size: 36px;
	}

	.slider-title {
		font-family: 'Opposit-Medium';
		/* background-color: rgb(255 255 255 / 80%); */
		min-width: 20vw;
		padding: 25px;
		padding-left: 25px;
		padding-bottom: 10px;
		margin-bottom: 10px;
		position: absolute;
		z-index: 1;
		left: 0px;
		bottom: 0;
		color: #fff;
		font-size: 38px;
	}
	video {
		width: 100vw;
		height: 100vh;
		object-fit: cover;
	}

	.image-logo {
		position: fixed;
		left: 25px;
		top: 30px;
		width: 177px;
		height: 4.75rem;
		z-index: 1;
		cursor: url(/images/home-cursor.png), auto;
	}

	.right-cursor {
		cursor: url(/images/right-cursor.svg), auto;
	}
	.left-cursor {
		cursor: url(/images/left-cursor.svg), auto;
	}

	@media screen and (max-width: 1200px) {
		.slider-title {
			bottom: 0;
			font-size: 2.25rem;
		}
		.paginator {
			font-size: 1.6rem;
			right: 20vw;
			bottom: 0;
			margin-bottom: -10px;
		}

		.slide {
			background-attachment: scroll;
			background-position: center !important;
		}

		.image-logo {
			width: 117px;
		}
	}

	@media screen and (max-width: 600px) {
		.slider-title {
			font-size: 1rem;
			width: 5vw;
		}

		.paginator {
			font-size: 1rem;
			right: 5vw;
			margin-right: 35px;
		}

		.slide {
			height: 60vw;
		}

		.text_slide {
			padding-top: 0px;
			font-size: 10px !important;
		}

		.text_slide_container {
			padding-top: 5px;
		}

		.text_title {
			padding-top: 5px;
			font-size: 24px;
		}
	}
</style>
