# PHP style guide

We aim to follow the [Symfony Coding Standards](https://symfony.com/doc/current/contributing/code/standards.html).

Also apply relevant PSR standards where not conflicting with the Symfony Coding Standards:

http://www.php-fig.org/psr/ (1, 2 and 4)

Use `composer` for package management. It's pretty much THE standard.


## PHP versions

Code should run on PHP v5.5 at the time of writing. This means you can and should:

- Always use short array syntax `[1, 2, 3]` in preference to long `array(1, 2, 3)`.
- Use generators (`yield`) to simplify iterators.
- Use `finally` blocks to close resources.
- You can use traits to reuse code (though not always ‘should’).

Support for v5.5 will be dropped on 10 Jul 2016, and we should have all systems upgraded to v5.6 by then.

See: https://secure.php.net/supported-versions.php


## PHP templates

If you are writing templates in raw PHP it is helpful to write in a slightly different style to preserve clarity
when switching between HTML and PHP.

1. Always use the alternative form for PHP control statements.

    ```php
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


2. Let PHP and HTML blocks each have their own level of indentation.

    ```php
    <div class="truth">
        <?php if (true): ?>
            <blink>
                the truth is out there
            </blink>
        <?php endif; ?>
    </div>
    ```
    
    Avoid
    
    ```php
    <div class="truth">
    <?php if (true): ?>
        <blink>
            the truth is out there
        </blink>
    <?php endif; ?>
    </div>
    ```

3. Use the short echo syntax.

    ```php
    <p>this is much <?= strtoupper("better") ?></p>
    ```

    Avoid

    ```php
    <p>this is much <?php echo strtoupper("worse"); ?></p>
    ```

4. Don't start accessing the database within a template, sending emails, or any other weird shit like that.

    ```php
    <?php
    $conn = mysql_connect('foobar', ...);
    ...
    $result = mysql_query('select * from widgets where id=' . $_POST['security_hole'], $conn);
    ?>
    
    <?php while ($row = mysql_fetch_assoc($result)): ?>
        <?= $row['name'] ?>
    <?php endwhile; ?>
    ```
    
    You shouldn't be using `mysql_` anyway because it is deprecated, but that's a whole other story.
