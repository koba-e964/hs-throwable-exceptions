# :diamonds: throwable-exceptions :diamonds:
[![Build Status](https://travis-ci.org/aiya000/hs-throwable-exceptions.svg?branch=master)](https://travis-ci.org/aiya000/hs-throwable-exceptions)
[![Hackage](https://img.shields.io/hackage/v/lens.svg)](https://hackage.haskell.org/package/throwable-exceptions)

throwable-exceptions gives the exception's value constructors for your haskell project :dog:


## Document is available in here

- [throwable-exceptions - Hackage](https://hackage.haskell.org/package/throwable-exceptions)
- [the simulation of the use case](https://github.com/aiya000/hs-throwable-exceptions/blob/master/test/Control/Exception/ThrowableTest.hs)


# :muscle: Why we should use this ? :muscle:
The situation that we want to throw the specific exception is happened frequently,
but many general exceptions are not given by base.

For example, [IOException](https://hackage.haskell.org/package/base-4.9.1.0/docs/Control-Exception.html#t:IOException)'s value constructor is not given.

```haskell
import Control.Exception.Safe (MonadThrow, throwM, IOException(..), try, Exception, SomeException)
import Control.Monad.IO.Class (MonadIO, liftIO)
import Control.Monad.Trans.Either (runEitherT)

readFileOrError :: (MonadThrow m, MonadIO m) => FilePath -> m String
readFileOrError path = do
  xOrErr <- liftIO . try $ readFile path
  case xOrErr of
    Left e -> throwM . IOException $ show (e :: SomeException)
    Right a -> return a

main :: IO ()
main = do
  xOrErr <- runEitherT $ readFileOrError "Main.hs"
  case xOrErr of
    Left e -> putStrLn $ "oops: " ++ show (e :: SomeException)
    Right a -> putStrLn a

-- Result output vv
-- Data constructor not in scope: IOException :: [Char] -> e1
```

But you have not to define a specific exception yourself
if you use this :muscle:


# :+1:
PR is welcome !
