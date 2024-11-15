<template>
<!--  <div-->
<!--    ref="stickySectionsContainer"-->
<!--    class="max-w-6xl mx-auto lg:min-h-[var(&#45;&#45;stick-items)] px-5 relative"-->
<!--  >-->
  <div
      ref="stickySectionsContainer"
      class="max-w-6xl mx-auto px-5 relative"
  >
<!--    <div-->
<!--      class="static lg:absolute top-8 left-1/2 translate-x-0 lg:min-h-[var(&#45;&#45;stick-items)] lg:-translate-x-1/2 mb-8 lg:mb-0"-->
<!--    >-->
    <div
        class="static top-8 translate-x-0 lg:min-h-[var(--stick-items)] mb-8 lg:mb-0">
            <h2
              class="text-center mx-auto max-w-xl text-4xl sm:text-5xl font-semibold tracking-tight text-gray-900 font-display mb-8"
            >
              Unlock Boundless Development Magic
            </h2>
            <p>
              Experience the limitless potential of Evolve, a suite of Laravel packages that bring agile CRUD operations, effortless API endpoints,
              and an elegant user interface to your applications. EvolveCORE is the essential guide, built to empower your app with rich features like
              model-defined validation, infinitely nested filtering, and event-driven hooks. With the optional EvolveAPI and EvolveUI packages, bring
              added realms of interactivity and RESTful accessibility to your projects.
            </p>

            <h2
                class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24"
            >Available Components:</h2>

            <h3
                class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8"
            >EvolveCORE (Foundation / Prerelease)</h3>
            <ul>
              <li class="mb-2"><strong>Model-Defined Configuration</strong>: Models define their own validation, displayable fields, searchable, and sortable fields.</li>
              <li class="mb-2"><strong>Event Hooks on Controllers</strong>: Controllers trigger <code>beforeCreate</code>, <code>afterUpdate</code>, and similar hooks, for seamless action customization.</li>
              <li class="mb-2"><strong>Dynamic Filtering & Sorting</strong>: Supports traditional operators (<code>&lt;</code>, <code>&gt;</code>, <code>=</code>) and string-based (<code>lt</code>, <code>gt</code>, <code>eq</code>) with multi-level relationship filtering.</li>
              <li class="mb-2"><strong>Role-Based Permissions</strong>: Integrates with Spatie Permissions, registering policies dynamically based on model configuration.</li>
              <li class="mb-2"><strong>Automatic Validation Rules</strong>: Generates validation rules from database constraints, with model overrides as needed.</li>
            </ul>

            <h3
                class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8"
            >EvolveAPI (Optional Free Extension)</h3>
            <ul>
              <li><strong>Dynamic API Routes</strong>: Creates RESTful endpoints with filtering, sorting, and pagination.</li>
              <li><strong>Standardized JSON Responses</strong>: Consistent, customizable success and error formats.</li>
              <li><strong>Batch Operations</strong>: Supports batch create, update, and delete requests.</li>
              <li><strong>Role-Based Access Control</strong>: Automatically enforces evolveCORE permissions on each request.</li>
              <li><strong>Swagger Documentation (Optional)</strong>: Generate API docs with Swagger for seamless endpoint navigation.</li>
            </ul>

            <h3
                class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8"
            >EvolveUI (Optional Free Extension)</h3>
            <ul>
              <li><strong>Livewire-Based CRUD Interface</strong>: Fully-interactive Livewire components for index, create, edit, and show actions.</li>
              <li><strong>Dynamic Navigation Menu</strong>: Builds menus from model configurations, showing only accessible options based on role.</li>
              <li><strong>DaisyUI Styling</strong>: Consistent styling across all components, with Jetstream compatibility.</li>
              <li><strong>Model-Defined Fields</strong>: Respects searchable, sortable, and filterable fields defined in each model’s configuration.</li>
            </ul>

            <p>
              Each package is built to be flexible, allowing you to expand functionality as your application grows.
              Transform your app with Evolve's effortless customization, bringing agile and powerful development tools to your fingertips.
            </p>
          </div>
      <!--    <div class="lg:sticky lg:top-0 lg:h-screen space-y-12 lg:space-y-0">-->
<!--      <HomeStickyFeaturesSection-->
<!--        title="Authentication"-->
<!--        heading="Easy Login and User Management"-->
<!--        description="We're all famililar with the pain of writing auth logic and the amount of use cases that come with it. I wanted to make it stupid simple and easy to use."-->
<!--      >-->
<!--        <HomeStickyFeaturesVideo-->
<!--          src="https://essentials.supersaas.dev/essentials_passkeys.mp4"-->
<!--        />-->
<!--      </HomeStickyFeaturesSection>-->
<!--      <HomeStickyFeaturesSection-->
<!--        title="Payments"-->
<!--        heading="Simple Billing Management"-->
<!--        description="Supports Stripe and Lemonsqueezy with a simple and easy to use unified composable."-->
<!--      >-->
<!--        <HomeStickyFeaturesVideo-->
<!--          src="https://essentials.supersaas.dev/essentials_posts-subs.mp4"-->
<!--        />-->
<!--      </HomeStickyFeaturesSection>-->
<!--      <HomeStickyFeaturesSection-->
<!--        title="Profile Settings"-->
<!--        heading="Let users update their profile info"-->
<!--        description="A simple way for users to manage and update their profile settings. It's not just functions, it looks beautiful."-->
<!--      >-->
<!--        <HomeStickyFeaturesVideo-->
<!--          src="https://essentials.supersaas.dev/essentials_settings.mp4"-->
<!--        />-->
<!--      </HomeStickyFeaturesSection>-->
<!--      <HomeStickyFeaturesSection-->
<!--        title="File Storage"-->
<!--        heading="Supports S3, Local file storage and NuxtHub"-->
<!--        description="A unified composable for file storage with demos for Client Upload, Server upload and more."-->
<!--      >-->
<!--        <HomeStickyFeaturesVideo-->
<!--          src="https://essentials.supersaas.dev/essentials_file-storage.mp4"-->
<!--        />-->
<!--      </HomeStickyFeaturesSection>-->
<!--      <HomeStickyFeaturesSection-->
<!--        title="Admin"-->
<!--        heading="A beautiful admin panel"-->
<!--        description="Manage users, ban users, manage subscriptions. View waitlist submissions. View all your transactions from Stripe or Lemonsqueezy. All in one place"-->
<!--      >-->
<!--        <HomeStickyFeaturesVideo-->
<!--          src="https://essentials.supersaas.dev/essentials_admin.mp4"-->
<!--        />-->
<!--      </HomeStickyFeaturesSection>-->
<!--    </div>-->
  </div>
</template>

<script setup>
const stickySectionsContainer = ref(null);

onMounted(() => {
  const stickySectionsInstance = new StickySections(
    stickySectionsContainer.value
  );
  window.addEventListener("scroll", stickySectionsInstance.onScroll);

  onBeforeUnmount(() => {
    window.removeEventListener("scroll", stickySectionsInstance.onScroll);
  });
});

class StickySections {
  constructor(containerElement) {
    this.container = {
      el: containerElement,
      height: 0,
      top: 0,
      bottom: 0,
    };
    this.sections = Array.from(this.container.el.querySelectorAll("section"));
    this.viewportTop = 0;
    this.activeIndex = 0;
    this.scrollValue = 0; // Scroll value of the sticky container
    this.onScroll = this.onScroll.bind(this);
    this.initContainer = this.initContainer.bind(this);
    this.handleSections = this.handleSections.bind(this);
    this.remapValue = this.remapValue.bind(this);
    this.init();
  }

  onScroll() {
    this.handleSections();
  }

  initContainer() {
    this.container.el.style.setProperty(
      "--stick-items",
      `${this.sections.length + 1}00vh`
    );
    this.container.el.classList.add("[&_*]:!transition-none");
    setTimeout(() => {
      this.container.el.classList.remove("[&_*]:!transition-none");
    }, 1);
  }

  handleSections() {
    this.viewportTop = window.scrollY;
    this.container.height = this.container.el.clientHeight;
    this.container.top = this.container.el.offsetTop;
    this.container.bottom = this.container.top + this.container.height;

    if (this.container.bottom <= this.viewportTop) {
      // The bottom edge of the stickContainer is above the viewport
      this.scrollValue = this.sections.length + 1;
    } else if (this.container.top >= this.viewportTop) {
      // The top edge of the stickContainer is below the viewport
      this.scrollValue = 0;
    } else {
      // The stickContainer intersects with the viewport
      this.scrollValue = this.remapValue(
        this.viewportTop,
        this.container.top,
        this.container.bottom,
        0,
        this.sections.length + 1
      );
    }
    this.activeIndex =
      Math.floor(this.scrollValue) >= this.sections.length
        ? this.sections.length - 1
        : Math.floor(this.scrollValue);

    this.sections.forEach((section, i) => {
      if (i === this.activeIndex) {
        section.style.setProperty("--stick-visibility", "1");
        section.style.setProperty("--stick-scale", "1");
      } else {
        section.style.setProperty("--stick-visibility", "0");
        section.style.setProperty("--stick-scale", ".8");
      }
    });
  }

  // This function remaps a value from one range to another range
  remapValue(value, start1, end1, start2, end2) {
    const remapped =
      ((value - start1) * (end2 - start2)) / (end1 - start1) + start2;
    return remapped > 0 ? remapped : 0;
  }

  init() {
    this.initContainer();
    this.handleSections();
  }
}
</script>
