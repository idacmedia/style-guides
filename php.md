# PHP style guide

We aim to follow the [Symfony Coding Standards](https://symfony.com/doc/current/contributing/code/standards.html).

And also relevant PSR standards where not conflicting with the Symfony Coding Standards:

http://www.php-fig.org/psr/ (1, 2 and 4)

And of course, using `composer` for package management is pretty much THE standard.


## PHP versions

Code should run on PHP v5.5 at the moment.

Support for v5.5 will be dropped on 10 Jul 2015, and we should have all systems upgraded to v5.6 by then.

See: https://secure.php.net/supported-versions.php


## PHP templates

If you are writing templates in raw PHP it is helpful to write in a slightly different style to preserve clarity
when switching between HTML and PHP.

1. Always use the alternative form for PHP control statements.

```
<?php foreach (range(0, 5) as $i): ?>
    <p><?= $i ?></p>
<?php endforeach; ?>

<?php if (true): ?>
    <blink>the truth is out there</blink>
<?php endif; ?>

<?php
// Exception: in a multiline PHP block revert to normal style:
if (true) {
    echo 'the truth is out there';
}
?>

```


2. Always indent PHP and HTML blocks alike.

```
<div class="truth">
    <?php if (true): ?>
        <blink>
            the truth is out there
        </blink>
    <?php endif; ?>
</div>
```

3. Use the short echo syntax.

```
<p>this is much <?= strtoupper("better") ?></p>

<!-- rather than -->

<p>this is much <?php echo strtoupper("better"); ?></p>
```

4. Don't start accessing the database within a template, sending emails, or any other weird shit like that.
