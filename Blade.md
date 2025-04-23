# What is Blade?
Blade is Laravel’s powerful, lightweight templating engine used to create views (the UI part of an MVC app). It's used for mixing HTML with PHP logic cleanly and efficiently.
Instead of writing raw PHP like this: <?php echo $title; ?>
In Blade, you just write: {{ $title }}

<br>
<code style="font-size:35px; padding:10px 0px;margin-top:10px;">Blade Basics: Displaying Data</code>

### 1.Outputting data ({{ }}, {!! !!})

```
<!-- resources/views/welcome.blade.php -->
<!-- This is a standard HTML structure -->
<!DOCTYPE html>
<html>
<head>
    <title>My First Blade Page</title>
</head>
<body>

    <!-- This outputs the value of the $name variable -->
    <h1>Hello, {{ $name }} </h1>

     <!-- You can also use escaped HTML (safe) -->
    <p>Your bio: {{ $bio }}</p>

     <!-- If you want to show raw HTML (not escaped), use: -->
    {!! $htmlContent !!}

</body>
</html>
```



## 2.Conditionals (@if, @unless, @isset, @empty, @auth, @guest, @switch)
```
<!-- Conditional logic -->
@if($age >= 18)
    <p>You are an adult.</p>
@elseif($age > 12)
    <p>You are a teenager.</p>
@else
    <p>You are a child.</p>
@endif

<!-- Check if a variable is set -->
@isset($name)
    <p>Name is set: {{ $name }}</p>
@endisset

<!-- Check if a variable is empty -->
@empty($hobby)
    <p>No hobbies listed.</p>
@endempty
```


## 3.Loops (@for, @foreach, @forelse, $loop)
```
<!-- For Loop -->
@for($i = 0; $i < 5; $i++)
    <p>Loop index: {{ $i }}</p>
@endfor

<!-- Foreach Loop -->
@foreach($items as $item)
    <p>Item: {{ $item }}</p>
@endforeach

<!-- Forelse: handles empty arrays too -->
@forelse($tasks as $task)
    <p>{{ $task }}</p>
@empty
    <p>No tasks found!</p>
@endforelse

<!-- While Loop -->
@php $counter = 1; @endphp
@while($counter <= 3)
    <p>Counter: {{ $counter }}</p>
    @php $counter++; @endphp
@endwhile
```


## 4.Blade Layouts and Sections:

### Main Layout <code style="color:red">app.blade.php</code> :

``` 
<!-- resources/views/layouts/app.blade.php -->

<!DOCTYPE html>
<html>
<head>
    <title>@yield('title')</title> <!-- Placeholder for title -->
</head>
<body>

    <!-- Placeholder for main content -->
    <div class="container">
        @yield('content')
    </div>

</body>
</html>
```

### Extend the layout in a child view <code style="color:red">home.blade.php</code>: 
```
<!-- resources/views/home.blade.php -->

@extends('layouts.app') <!-- Use the base layout -->

@section('title', 'Home Page') <!-- Set the title -->

@section('content') <!-- Fill in the content section -->
    <h1>Welcome to the Home Page</h1>
    <p>This content is injected into the layout.</p>
@endsection
```



## 5.Blade Components (Reusable UI blocks) :
### Create a component  : 
```
<!-- resources/views/components/alert.blade.php -->

<div style="color: red;">
    <!-- This will output the slot content passed to this component -->
    {{ $slot }}
</div>
```

### How to Use Componants In Blade :
```
<x-alert>
    This is an important message!
</x-alert>
```


# 6.Blade Includes (include other views)
```
<!-- resources/views/partials/header.blade.php -->
<h2>This is the header</h2>

<!-- In main view -->
@include('partials.header')

```

# 7.Blade Comments
```
{{-- This is a Blade comment. It will not show in the HTML source. --}}
```

# 8.Blade Directives: Custom logic helpers

```
@auth
    <p>You're logged in!</p>
@endauth

@guest
    <p>You're not logged in.</p>
@endguest

@switch($role)
    @case('admin')
        <p>You are an admin</p>
        @break
    @case('user')
        <p>You are a user</p>
        @break
    @default
        <p>Unknown role</p>
@endswitch
```
<code style="background-color:#5bc0de;color:black;">This gives you the complete Blade toolkit to build full views from scratch.</code>

<br>
<br>

<code style="font-size:25px; padding:10px 0px;margin-top:10px;text-align:center;width:100%;">Advanced Blade Topics (Blade Hero Level)</code>

<br>

## 1.Passing Data to Views (Controller → Blade)

### In your controller:
```
1- return view('profile', ['name' => 'John']);

/********************************************/
/********************************************/

$name = John;
2- return view('profile', compact("name"));
```

### Getting and Display Data In Blade:
```
<!-- In Blade view -->
<h1>Hello, {{ $name }}</h1>
```

## 2. Blade Layout Stacks (@push, @stack)

**Useful when you want child views to add scripts/styles to specific sections in the layout.**

### In your main layout file:
```
<!-- Add this in your layout inside the <head> -->
@stack('styles')

<!-- Add this before </body> -->
@stack('scripts')
```

### In your child view:
```
@push('styles')
    <link rel="stylesheet" href="custom.css">
@endpush

@push('scripts')
    <script src="app.js"></script>
@endpush
```

## 3.Blade Component with Attributes & Slots

<code style="color:white;">Command to create a new Component php artisan make:component Alert</code>

### Component with named slots: 
```
<!-- resources/views/components/alert.blade.php -->

<div class="alert alert-{{ $type }}">
    {{ $message }}
    <div>
        {{ $slot }}
    </div>
</div>
```
### Controller : 
```
class Alert extends Component
{
    public $type;
    public $message;

    /**
     * Create a new component instance.
     */
    public function __construct($type, $message)
    {
        $this->type = $type; # must found in constructor to define in blade file
        $this->message = $message; # must found in constructor to define in blade file
    }

    /**
     * Get the view / contents that represent the component.
     */
    public function render(): View|Closure|string
    {
        return view('components.alert');
    }
}
```
### Usage:
```
<x-alert type="success" message="Operation successful!" >
    Thank You
</x-alert>
```
<code style="background-color:#5bc0de;color:black;">(x-card) => card is name of the blade file</code>
