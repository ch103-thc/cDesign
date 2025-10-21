<script>
	import Icon from '@iconify/svelte';
	import { onMount, onDestroy } from 'svelte';

	// Fractal tree settings
	let fractalCanvas;
	let fractalAngle = $state(0.5);

	onMount(() => {
		// Get the canvas element
		fractalCanvas = document.getElementById('fractalCanvas');
		if (!fractalCanvas) return;

		const ctx = fractalCanvas.getContext('2d');

		// Set canvas dimensions
		const resizeCanvas = () => {
			fractalCanvas.width = window.innerWidth;
			fractalCanvas.height = 220; // Increased height to show all branches
			drawFractalTree(ctx);
		};

		// Handle window resize
		window.addEventListener('resize', resizeCanvas);
		resizeCanvas();

		// Animation loop
		function animate() {
			// Animate the angle slightly for a gentle swaying effect
			fractalAngle = 0.5 + Math.sin(Date.now() * 0.001) * 0.3;

			// Clear and redraw
			ctx.clearRect(0, 0, fractalCanvas.width, fractalCanvas.height);
			drawFractalTree(ctx);

			requestAnimationFrame(animate);
		}

		animate();

		return () => {
			window.removeEventListener('resize', resizeCanvas);
		};
	});

	// Draw the fractal tree
	function drawFractalTree(ctx) {
		const startX = fractalCanvas.width / 2;
		const startY = fractalCanvas.height - 30; // Start higher from the bottom
		const branchLength = 40; // Shorter initial branch

		// Start the recursive drawing
		drawBranch(ctx, startX, startY, branchLength, -Math.PI / 2, 8);
	}

	// Recursive function to draw branches
	function drawBranch(ctx, x, y, length, angle, depth) {
		if (depth <= 0) return;

		// Calculate end point
		const endX = x + Math.cos(angle) * length;
		const endY = y + Math.sin(angle) * length;

		// Draw the branch
		ctx.beginPath();
		ctx.moveTo(x, y);
		ctx.lineTo(endX, endY);
		ctx.strokeStyle = 'white';
		ctx.lineWidth = depth / 4;
		ctx.stroke();

		// Calculate new length
		const newLength = length * 0.8;

		// Draw right branch with a slightly wider angle
		drawBranch(ctx, endX, endY, newLength, angle - fractalAngle * 1.5, depth - 1);

		// Draw left branch
		drawBranch(ctx, endX, endY, newLength, angle + fractalAngle * 1.5, depth - 1);
	}

	// Split the text into parts
	const textParts = [
		{ text: 'hi, ', color: 'var(--lightest-slate)' },
		{ text: 'chloe', color: 'var(--green-bright)', fontWeight: 'bold' }, // Added fontWeight property
		{ text: ' here.', color: 'var(--lightest-slate)' }
	];

	// Initialize variables to track typing progress
	let currentPartIndex = $state(0);
	let currentCharIndex = $state(0);
	let displayParts = $state([
		{
			text: '',
			color: textParts[0].color,
			fontWeight: textParts[0].fontWeight
		}
	]);
	let cursorVisible = $state(true);
	let typingComplete = $state(false);

	onMount(() => {
		// Start typing animation
		const typingInterval = setInterval(() => {
			if (currentPartIndex < textParts.length) {
				const currentPart = textParts[currentPartIndex];

				if (currentCharIndex < currentPart.text.length) {
					// Add next character to the current part
					displayParts[currentPartIndex].text += currentPart.text[currentCharIndex];
					currentCharIndex++;
				} else {
					// Move to next part
					currentPartIndex++;
					currentCharIndex = 0;

					// If there are more parts to type, initialize the next part
					if (currentPartIndex < textParts.length) {
						displayParts.push({
							text: '',
							color: textParts[currentPartIndex].color,
							fontWeight: textParts[currentPartIndex].fontWeight
						});
						displayParts = displayParts; // Trigger reactivity in Svelte
					} else {
						// Typing is complete
						clearInterval(typingInterval);
						typingComplete = true;
					}
				}
			}
		}, 80); // Adjust speed of typing here

		// Cursor blinking animation
		setInterval(() => {
			cursorVisible = !cursorVisible;
		}, 500);
	});

	let slideInterval;

	onMount(() => {
		// Auto slide every 4 seconds
		slideInterval = setInterval(() => {
			nextSlide();
		}, 4000); // 4000 ms = 4 seconds

		return () => {
			clearInterval(slideInterval);
		};
	});

	onDestroy(() => {
		clearInterval(slideInterval);
	});

	let currentIndex = $state(0);

	const projects = [
		{
			id: 1,
			title: 'BuiltSearch 3D Library',
			image: 'img/3dlib.png',
			description1:
				'A platform dedicated to providing a free collection of 3D assets specifically for the built environment, such as architecture, engineering, and construction professionals.',
			description2: 'Svelte, JavaScript & SCSS',
			url: 'https://3d.builtsearch.com/'
		},
		{
			id: 2,
			title: 'Digital Revivo',
			image: 'img/digital-revivo.png',
			description1:
				'A sleek, product-focused storefront for K-Beauty enthusiasts featuring curated products, interactive content, and dynamic visuals. Designed to inspire discovery and drive conversions through affiliate links.',
			description2: 'Svelte, JavaScript & SCSS',
			url: 'https://digitalrevivo.com/'
		},
		{
			id: 3,
			title: 'Nomora',
			image: 'img/nomora.png',
			description1:
				'A sleek landing page dedicated to showcasing a curated collection of modern travel clothing, combining performance-driven features with lifestyle-focused branding for travelers and urban explorers.',
			description2: 'Svelte, JavaScript & SCSS',
			url: 'https://nomora-green.vercel.app/'
		}
	];

	const cards = [
		{
			id: 1,
			fileIcon: 'mdi:folder-outline',
			header: 'Incorvia',
			paragraph:
				'Incorvia is a powerful platform designed to help businesses grow and scale effortlessly. Designed with the user in mind, it offers flexible pricing plans to fit different business needs, seamless integrations with over 100+ tools, and a clean, intuitive interface that’s easy to navigate. Its user-centered design and scalable features ensure a smooth and effective user experience.',
			footer: 'Svelte, JavaScript & SCSS',
			url: 'https://incorvia.vercel.app/'
		},
		{
			id: 2,
			fileIcon: 'mdi:folder-outline',
			header: 'Innospace',
			paragraph:
				'This website is an e-commerce platform dedicated to selling ergonomic office furniture and accessories designed to enhance productivity and comfort in both home and team office environments. The site features a modern, user-friendly design with rich multimedia elements such as autoplay videos and interactive product carousels.',
			footer: 'Svelte, JavaScript & SCSS',
			url: 'https://innospace-nine.vercel.app/'
		},
		{
			id: 3,
			fileIcon: 'mdi:folder-outline',
			header: 'Aurest',
			paragraph:
				'The Aurest® website is a modern e-commerce site for organic sleep products, with easy navigation, interactive product displays, and badges for best sellers and new arrivals which enhance user engagement. Educational content, integrated social feeds and accessible support options further enrich the user experience, blending aesthetics with functional design focused on sustainability and comfort.',
			footer: 'Svelte, JavaScript & SCSS',
			url: 'https://ch103-thc.github.io/aurest/'
		},
		{
			id: 4,
			fileIcon: 'mdi:folder-outline',
			header: 'Noure',
			paragraph:
				'A modern, content-rich landing page for a beauty and wellness platform. Editorial storytelling is seamlessly integrated with expert tips and curated insights, all presented through a user-friendly interface that encourages exploration. The design invites visitors to effortlessly navigate categories, discover trends, and access trusted recommendations, creating an engaging and empowering self-care experience.',
			footer: 'Svelte, JavaScript & SCSS',
			url: 'https://noure.vercel.app/'
		},
		{
			id: 5,
			fileIcon: 'mdi:folder-outline',
			header: 'Esther Sinclair',
			paragraph:
				'This website is a professional portfolio showcasing a front-end engineer’s skills in creating accessible and pixel-perfect web experiences. It highlights their technical expertise, key projects, and published articles, emphasizing a balance between thoughtful design and strong engineering. The site also includes links to social profiles and a resume, offering a clear overview of their professional background and work.',
			footer: 'Svelte, JavaScript & SCSS',
			url: 'https://esther-sinclair.vercel.app/'
		},
		{
			id: 6,
			fileIcon: 'mdi:folder-outline',
			header: 'Echo Me',
			paragraph:
				'This website is an interactive, terminal-style portfolio that lets visitors explore a software developer’s skills, projects, and contact details through simple text commands. Users can navigate the portfolio by typing commands like about, skills, projects, and contact to learn more about the developer’s background, technical abilities, and completed or ongoing projects.',
			footer: 'Svelte, JavaScript & SCSS',
			url: 'https://echo-me-eta.vercel.app/'
		}
	];

	function nextSlide() {
		currentIndex = (currentIndex + 1) % projects.length;
	}

	function prevSlide() {
		currentIndex = (currentIndex - 1 + projects.length) % projects.length;
	}

	function goToSlide(index) {
		currentIndex = index;
	}
</script>

<main>
	<!-- Fractal tree canvas added above the intro heading -->
	<div class="fractal-container">
		<canvas id="fractalCanvas"></canvas>
	</div>
	<div id="intro" class="intro">
		<div class="intro-title">
			{#each displayParts as part}
				<span
					class="typing-text"
					style="color: {part.color}; font-weight: {part.fontWeight || 'normal'}">{part.text}</span
				>
			{/each}
			<span class="cursor-blinking" class:fade-out={!cursorVisible}>|</span>
		</div>
		<div class="intro-subtitle">I create stuff sometimes.</div>
		<div class="intro-desc">
			I'm a web developer from Singapore, passionate about creating reliable and user-friendly web
			applications. I enjoy designing and developing fun, engaging digital experiences that
			prioritise user needs, optimise performance, and bring creative ideas to life.
		</div>
		<!-- <p>
            I've worked on production systems used by a broad audience and
            helped deliver features that improve everyday experiences on the
            web.
        </p> -->
		<button class="intro-btn">
			<Icon icon="mdi:instagram" width="24" height="24" />
			<div class="intro-text">Say hi!</div>
		</button>
	</div>

	<div id="about" class="about">
		<div class="about-content">
			<div class="section-header">
				<h3>/ about me</h3>
				<hr />
			</div>
			<p>
				Hi, I’m Chloe! I’m a <span class="highlight"> Frontend Web Developer </span> passionate about
				building and managing the front-end of websites and web applications that drive product success.
			</p>
			<p>
				I embrace challenges with curiosity and believe that all design should be user-centered. I
				strive to <span class="highlight"> add value </span> to my projects, contribute positively to
				my work environment, and enjoy working on projects that are both challenging and innovation driven.
			</p>
			<p>
				Feel free to connect with or follow me on <span class="highlight">LinkedIn </span>
				and <span class="highlight">Instagram</span>. You may also know more about me through my
				profiles there!
			</p>
			<!-- <p>
                I'm always on the lookout for new job opportunities where I can
                contribute, learn, and grow. If you have an opportunity that
                matches my skills and experience, don't hesitate to contact me!
            </p> -->
			<p>Here are some technologies I have been working with:</p>
			<div class="tech-list">
				<div class="tech-item">
					<Icon icon="line-md:play" width="16" height="16" />
					<span>HTML</span>
				</div>
				<div class="tech-item">
					<Icon icon="line-md:play" width="16" height="16" />
					<span>CSS</span>
				</div>
				<div class="tech-item">
					<Icon icon="line-md:play" width="16" height="16" />
					<span>JavaScript</span>
				</div>
				<div class="tech-item">
					<Icon icon="line-md:play" width="16" height="16" />
					<span>Svelte</span>
				</div>
				<div class="tech-item">
					<Icon icon="line-md:play" width="16" height="16" />
					<span>SCSS</span>
				</div>
				<div class="tech-item">
					<Icon icon="line-md:play" width="16" height="16" />
					<span>UI/UX Design</span>
				</div>
				<div class="tech-item">
					<Icon icon="line-md:play" width="16" height="16" />
					<span>Figma</span>
				</div>
			</div>
			<p>
				Outside of work, I enjoy spending time playing the piano, which allows me to unwind and
				express myself through music.
			</p>
		</div>
		<div class="about-image">
			<img src="img/profile.png" alt="Chloe Teo" />
		</div>
	</div>

	<div id="projects" class="projects">
		<div class="projects-content">
			<div class="section-header">
				<h3>/ projects</h3>
				<hr />
			</div>
			<div class="carousel">
				<div class="carousel-wrapper" style="transform: translateX(-{currentIndex * 100}%);">
					{#each projects as project (project.id)}
						<div class="carousel-slide">
							<img src={project.image} alt={project.title} />
							<div class="overlay"></div>
							<div class="carousel-caption">
								<h2>{project.title}</h2>
								<p>{project.description1}</p>
								<div class="description2">
									{project.description2}
								</div>
								<div class="eye-icon">
									<a href={project.url} target="_blank" rel="noopener noreferrer">
										<Icon icon="mdi:eye" width="20" height="20" />
									</a>
								</div>
							</div>
						</div>
					{/each}
				</div>

				<button class="carousel-arrow left" onclick={prevSlide}
					><Icon icon="mingcute:left-fill" width="40" height="40" /></button
				>
				<button class="carousel-arrow right" onclick={nextSlide}
					><Icon icon="mingcute:right-fill" width="40" height="40" /></button
				>

				<div class="carousel-indicators">
					{#each projects as _, idx}
						<span class:active={idx === currentIndex} onclick={() => goToSlide(idx)}></span>
					{/each}
				</div>
			</div>
			<div class="cards-section">
				<div class="cards-grid">
					{#each cards as card (card.id)}
						<div class="card">
							<div class="card-header">
								<div class="file-icon">
									<Icon icon={card.fileIcon} width="24" height="24" />
								</div>
								<div class="top-right-icon">
									<a href={card.url} target="_blank" rel="noopener noreferrer">
										<Icon icon="mdi:eye" width="20" height="20" />
									</a>
								</div>
							</div>
							<div class="card-content">
								<h3 class="card-title">{card.header}</h3>
								<p class="card-paragraph">{card.paragraph}</p>
								<div class="card-footer">{card.footer}</div>
							</div>
						</div>
					{/each}
				</div>
			</div>
		</div>
	</div>

	<div id="contact" class="contact">
		<div class="contact-content">
			<h1 class="contact-title">Get in Touch</h1>
			<p class="contact-desc">
				Let's Build Something Amazing Together! Have an idea in mind? I'd love to hear about your
				project and help bring it to life. Drop me a message, and let's get started!
			</p>

			<!-- <form action="#" class="contact-form">
                <div class="form-group">
                    <input
                        type="text"
                        id="name"
                        name="name"
                        placeholder="Enter your full name"
                        required
                    />
                </div>
                <div class="form-group">
                    <input
                        type="email"
                        id="email"
                        name="email"
                        placeholder="Enter your email"
                        required
                    />
                </div>
                <div class="form-group">
                    <textarea
                        id="message"
                        name="message"
                        placeholder="Your message"
                        rows="5"
                        required
                    ></textarea>
                </div>
                <button type="submit" class="submit-btn">Send Message</button>
            </form> -->

			<div class="social-media">
				<!-- <p>Or contact me via</p> -->
				<div class="social-icons">
					<a href="https://www.linkedin.com/in/chloe-teo-8a6295277/" target="_blank">
						<Icon icon="mdi:linkedin" width="24" height="24" />
					</a>
					<a href="https://www.instagram.com/thc_c04/" target="_blank">
						<Icon icon="mdi:instagram" width="24" height="24" />
					</a>
				</div>
			</div>
		</div>
	</div>
</main>

<style lang="scss">
	// Add styles for the fractal container
	.fractal-container {
		width: 100%;
		height: auto;
		display: flex;
		justify-content: center;
		align-items: center;
		background-color: var(--navy);
		padding: 110px 0 0 0; // Increased top padding
		margin-bottom: 0;
	}

	#fractalCanvas {
		display: block;
		background-color: var(--navy);
		width: 100%;
		height: 220px; // Match the JS height
		margin-bottom: -100px; // Increased negative margin to maintain proximity to header
	}

	div.about,
	div.projects,
	div.contact {
		padding: 70px 10% 50px;
		background-color: var(--navy);
	}

	div.intro {
		padding: 200px 10% 50px;
		background-color: var(--navy);
	}

	.intro {
		padding: 4rem 2rem;
		text-align: center;

		.intro-title {
			font-size: 5rem;
			margin-top: -1rem;
			color: var(--lightest-slate);

			@media (max-width: 768px) {
				font-size: 3rem;
				margin-bottom: 1.5rem;
			}

			.typing-text {
				display: inline-block;
				white-space: pre;
			}

			.cursor-blinking {
				color: var(--green-bright);
				display: inline-block;
				margin-left: -1rem;
				transition: opacity 0.3s ease-in-out;
				opacity: 1;

				&.fade-out {
					opacity: 0;
				}
			}
		}

		.intro-subtitle {
			font-size: 3rem;
			margin-top: -2rem;
			color: var(--slate);

			@media (max-width: 768px) {
				font-size: 1.8rem;
			}
		}

		.intro-desc {
			font-size: 1.5rem;
			color: var(--slate);
			max-width: 600px;
			margin: 0 auto 2rem auto;
			line-height: 1.5;

			@media (max-width: 768px) {
				font-size: 1.1rem;
			}
		}

		@keyframes blink {
			0%,
			100% {
				opacity: 1;
			}
			50% {
				opacity: 0;
			}
		}

		.intro-btn {
			display: inline-flex;
			align-items: center;
			gap: 0.5rem;
			padding: 1rem 2rem;
			font-size: 1rem;
			color: var(--green-bright);
			background-color: transparent;
			border: 1px solid var(--green-bright);
			cursor: pointer;
			border-radius: 12px;
			transition: background-color 0.3s ease;

			@media (max-width: 768px) {
				font-size: 0.8rem;
				padding: 0.8rem 1.5rem;
			}

			.intro-text {
				font-size: 1.5rem;
				font-weight: bold;

				@media (max-width: 768px) {
					font-size: 1rem;
				}
			}

			&:hover {
				background-color: var(--lightest-navy);
			}
		}
	}

	.section-header {
		display: flex;
		align-items: center;
		gap: 1rem;
		margin-bottom: 1.5rem;

		@media (max-width: 768px) {
			hr {
				display: none;
			}
		}

		hr {
			flex-grow: 1;
			border: none;
			height: 1px;
			background-color: var(--lightest-navy);
			max-width: 300px;
		}

		h3 {
			font-size: 3rem;
			font-weight: bold;
			color: var(--lightest-slate);
			margin: 0 0 10px 0;
			white-space: nowrap;

			@media (max-width: 768px) {
				font-size: 2rem;
			}
		}
	}

	.about {
		display: flex;
		align-items: flex-start;
		justify-content: space-between;
		gap: 1rem;

		@media (max-width: 768px) {
			.about-image {
				display: none;
			}
		}

		.about-content {
			width: 60%;

			@media (max-width: 768px) {
				width: 100%;
			}

			p {
				font-size: 1.5rem;
				color: var(--slate);
				margin-bottom: 16px;
				line-height: 1.5;

				@media (max-width: 768px) {
					font-size: 1.1rem;
				}
			}

			.tech-list {
				display: grid;
				grid-template-columns: repeat(2, minmax(0, 1fr));
				margin-bottom: 1.5rem;
				max-width: 300px;

				.tech-item {
					display: flex;
					align-items: center;
					gap: 0.5rem;
					font-weight: 500;
					color: var(--green-bright);

					span {
						font-size: 1.1rem;
						color: var(--slate);
					}
				}
			}
		}

		.about-image img {
			width: 100%;
			max-width: 300px;
			margin-top: 6.5rem;
			border-radius: 10px;
		}
	}

	.projects {
		.carousel {
			position: relative;
			overflow: hidden;
			width: 100%;
			// max-width: 900px;
			margin: 0 auto;
			background-color: var(--navy);
			border-radius: 12px;

			@media (max-width: 768px) {
				display: none;
			}

			.carousel-wrapper {
				display: flex;
				transition: transform 0.5s ease-in-out;
				width: 100%;
			}

			.carousel-slide {
				min-width: 100%;
				height: 530px;
				position: relative;
				display: flex;
				flex-direction: column;
				justify-content: flex-end;
				align-items: center;
				padding-bottom: 40px;
				box-sizing: border-box;

				.overlay {
					position: absolute;
					top: 0;
					left: 0;
					width: 100%;
					height: 100%;
					background: rgba(10, 25, 47, 0.6); // Adjust alpha for transparency
					z-index: 2;
					border-radius: 12px;
					pointer-events: none; // Ensures caption buttons still work
				}

				img {
					position: absolute;
					width: 100%;
					height: 100%;
					object-fit: cover;
					border-radius: 12px;
					z-index: 1;
					box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
				}

				.carousel-caption {
					position: relative;
					z-index: 2;
					// background-color: rgba(10, 25, 47, 0.8);
					padding: 1rem 2rem;
					border-radius: 8px;
					text-align: center;
					max-width: 80%;

					h2 {
						font-size: 2rem;
						font-weight: bold;
						color: var(--lightest-slate);
						margin-bottom: 0.25rem;
					}

					p {
						margin: 0.25rem 0;
						font-size: 1.1rem;
						font-weight: bold;
						color: var(--lightest-slate);
						max-width: 500px;
						margin-left: auto;
						margin-right: auto;
						line-height: 1.5;
					}

					.description2 {
						margin: 0.25rem 0 0 0;
						font-size: 1.1rem;
						font-weight: bold;
						color: var(--green-bright);
					}

					.eye-icon a {
						color: var(--lightest-slate);
						display: inline-flex;
						align-items: center;
						text-decoration: none;
					}
				}
			}

			.carousel-arrow {
				position: absolute;
				top: 50%;
				transform: translateY(-50%);
				font-size: 2rem;
				color: var(--green-bright);
				background-color: transparent;
				border: none;
				padding: 0.5rem 1rem;
				cursor: pointer;
				z-index: 3;
				transition: background 0.3s ease;

				&.left {
					left: 20px;
				}

				&.right {
					right: 20px;
				}
			}
		}

		.carousel-indicators {
			position: absolute;
			bottom: 15px;
			left: 50%;
			transform: translateX(-50%);
			display: flex;
			gap: 7px;
			z-index: 3;

			span {
				width: 30px;
				height: 2.5px;
				border-radius: 2px;
				background-color: var(--light-slate);
				cursor: pointer;
				transition: background-color 0.3s ease;

				&.active {
					background-color: var(--green-bright);
				}
			}
		}

		.cards-section {
			margin-top: 50px;

			.cards-grid {
				display: grid;
				grid-template-columns: repeat(3, 1fr);
				gap: 10px;
				max-width: 1200px;
				margin: 0 auto;

				@media (max-width: 768px) {
					grid-template-columns: 1fr !important;
					gap: 15px;
				}

				.card {
					background-color: var(--light-navy);
					border-radius: 12px;
					padding: 25px;
					transition:
						transform 0.3s ease,
						box-shadow 0.3s ease,
						background-color 0.3s ease;

					&:hover {
						transform: translateY(-5px);
						box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
						background-color: var(--lightest-navy);
					}

					.card-header {
						display: flex;
						justify-content: space-between;
						align-items: center;
						margin-bottom: 20px;

						.file-icon {
							color: var(--green-bright);
						}

						.top-right-icon a {
							color: var(--lightest-slate);
							display: inline-flex;
							align-items: center;
							text-decoration: none;

							&:hover {
								color: var(--green-bright);
							}
						}
					}

					.card-content {
						.card-title {
							font-size: 1.4rem;
							font-weight: bold;
							color: var(--lightest-slate);
							margin-bottom: 12px;
						}

						.card-paragraph {
							font-size: 1rem;
							color: var(--slate);
							line-height: 1.6;
							margin-bottom: 15px;
						}

						.card-footer {
							font-size: 0.9rem;
							color: var(--green-bright);
							font-weight: 500;
						}
					}
				}

				// Responsive design
				@media (max-width: 768px) {
					grid-template-columns: repeat(2, 1fr);
					gap: 20px;
				}

				@media (max-width: 480px) {
					grid-template-columns: 1fr;
					gap: 15px;
				}
			}
		}
	}

	.contact {
		box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
		text-align: center;

		@media (max-width: 768px) {
			display: none;
		}

		.contact-content {
			width: 50%;
			max-width: 900px;
			margin: 0 auto;
			padding: 50px 0;
		}

		.contact-title {
			font-size: 2.5rem;
			margin-bottom: 10px;
			color: var(--lightest-slate);
		}

		.contact-desc {
			font-size: 1.1rem;
			color: var(--slate);
			margin-bottom: 30px;
		}

		.contact-form {
			display: flex;
			flex-direction: column;
			gap: 20px;

			.form-group {
				display: flex;
				flex-direction: column;

				input,
				textarea {
					padding: 10px;
					border: 1px solid #ccc;
					border-radius: 5px;
					box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
					transition: all 0.3s ease-in-out;

					&:focus {
						border-color: black;
						outline: none;
					}
				}

				textarea {
					height: 120px;
					resize: none;
				}
			}
		}

		.submit-btn {
			padding: 12px 20px;
			background-color: #3498db;
			color: #fff;
			font-size: 1.2rem;
			border: none;
			border-radius: 5px;
			cursor: pointer;
			transition: background-color 0.3s;

			&:hover {
				background-color: #2980b9;
			}
		}

		.social-media {
			margin-top: 30px;
			text-align: center;

			p {
				font-size: 1.1rem;
				color: #7f8c8d;
				margin-bottom: 15px;
			}

			.social-icons {
				display: flex;
				justify-content: center;
				color: var(--lightest-slate);
				gap: 20px;

				a {
					color: var(--lightest-slate);
					transition: color 0.3s ease;

					&:hover {
						color: var(--green-bright);
					}
				}
			}
		}
	}
</style>
